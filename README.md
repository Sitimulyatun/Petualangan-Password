# Petualangan-Password
import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { CheckCircle, XCircle, Lightbulb, ShieldCheck } from "lucide-react";

export default function PasswordAdventure() {
  const [step, setStep] = useState(1);
  const [score, setScore] = useState(0);
  const [feedback, setFeedback] = useState("");
  const [selected, setSelected] = useState(null);
  const [customPassword, setCustomPassword] = useState("");

  const questions = [
    {
      question: "Level 1: Pilih password yang paling kuat:",
      options: ["123456", "kucingku", "B3rb4g4!_S3m4ng4t"],
      answer: 2
    },
    {
      question: "Level 2: Pilih password yang paling aman:",
      options: ["sekolahku", "iloveyou", "R4h@s14nku!"],
      answer: 2
    },
    {
      question: "Level 3: Perbaiki password 'password123' agar lebih kuat:",
      options: ["password123", "P@ssw0rd!2025", "mypassword"],
      answer: 1
    }
  ];

  const handleAnswer = (index) => {
    const correct = questions[step - 1].answer;
    setSelected(index);
    if (index === correct) {
      setFeedback("✅ Benar! Password ini kuat.");
      setScore(score + 1);
    } else {
      setFeedback("❌ Salah. Password ini terlalu mudah ditebak.");
    }
  };

  const nextStep = () => {
    setSelected(null);
    setFeedback("");
    setStep(step + 1);
  };

  const checkCustomPassword = () => {
    const pwd = customPassword;
    const hasUpper = /[A-Z]/.test(pwd);
    const hasLower = /[a-z]/.test(pwd);
    const hasNumber = /[0-9]/.test(pwd);
    const hasSymbol = /[^A-Za-z0-9]/.test(pwd);
    const isLongEnough = pwd.length >= 8;

    const passed = [hasUpper, hasLower, hasNumber, hasSymbol, isLongEnough].filter(Boolean).length;

    if (passed === 5) {
      setFeedback("✅ Hebat! Password kamu sangat kuat.");
      setScore(score + 1);
    } else {
      setFeedback("❌ Password masih lemah. Gunakan huruf besar, kecil, angka, simbol & panjang minimal 8 karakter.");
    }
  };

  return (
    <div className="max-w-xl mx-auto p-6">
      <h1 className="text-3xl font-bold mb-6 text-center text-blue-700">🔐 Password Petualangan</h1>
      <Card className="shadow-xl">
        <CardContent className="p-6 space-y-4">
          {step <= questions.length && (
            <>
              <p className="text-lg font-semibold">{questions[step - 1].question}</p>
              <div className="space-y-2">
                {questions[step - 1].options.map((opt, idx) => (
                  <Button
                    key={idx}
                    variant={selected === idx ? "secondary" : "default"}
                    onClick={() => handleAnswer(idx)}
                    disabled={selected !== null}
                  >
                    {opt}
                  </Button>
                ))}
              </div>
              {feedback && (
                <div className="mt-4 text-base">
                  {selected === questions[step - 1].answer ? (
                    <div className="text-green-600 flex items-center gap-2">
                      <CheckCircle /> {feedback}
                    </div>
                  ) : (
                    <div className="text-red-600 flex items-center gap-2">
                      <XCircle /> {feedback}
                    </div>
                  )}
                </div>
              )}
              {selected !== null && (
                <Button className="mt-4" onClick={nextStep}>Lanjut ke Level {step + 1}</Button>
              )}
            </>
          )}

          {step > questions.length && (
            <>
              <p className="text-lg font-semibold">Level 4: Buat passwordmu sendiri yang kuat!</p>
              <Input
                type="text"
                placeholder="Tulis password kamu di sini"
                value={customPassword}
                onChange={(e) => setCustomPassword(e.target.value)}
              />
              <Button onClick={checkCustomPassword}>Cek Password</Button>
              {feedback && (
                <div className={`mt-4 text-${feedback.startsWith("✅") ? "green" : "red"}-600 flex items-center gap-2`}>
                  {feedback.startsWith("✅") ? <CheckCircle /> : <XCircle />}
                  {feedback}
                </div>
              )}
              {!feedback.startsWith("✅") && feedback && (
                <div className="flex items-center gap-2 mt-2 text-yellow-600">
                  <Lightbulb />
                  <span>Tips: Kombinasi huruf besar, kecil, angka, simbol & panjang minimal 8 karakter.</span>
                </div>
              )}
              <div className="mt-6 text-lg font-bold text-center text-indigo-700">
                <ShieldCheck className="inline mr-2" />Skor Akhir: {score} / {questions.length + 1}
              </div>
            </>
          )}
        </CardContent>
      </Card>
    </div>
  );
}
