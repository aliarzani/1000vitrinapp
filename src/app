import React, { useState } from "react";
import {
  View,
  Text,
  TextInput,
  Picker,
  StyleSheet,
  Modal,
  Alert,
  TouchableOpacity,
} from "react-native";

// Utility function to convert numbers to Persian words
const numberToPersianWords = (num) => {
  if (num === 0) return "صفر تومان";

  const units = ["", "یک", "دو", "سه", "چهار", "پنج", "شش", "هفت", "هشت", "نه"];
  const teens = [
    "ده",
    "یازده",
    "دوازده",
    "سیزده",
    "چهارده",
    "پانزده",
    "شانزده",
    "هفده",
    "هجده",
    "نوزده",
  ];
  const tens = [
    "",
    "ده",
    "بیست",
    "سی",
    "چهل",
    "پنجاه",
    "شصت",
    "هفتاد",
    "هشتاد",
    "نود",
  ];
  const hundreds = [
    "صد",
    "دویست",
    "سیصد",
    "چهارصد",
    "پانصد",
    "ششصد",
    "هفتصد",
    "هشتصد",
    "نهصد",
  ];

  let words = [];

  const billion = Math.floor(num / 1_000_000_000);
  if (billion > 0) {
    words.push(numberToPersianWords(billion) + " میلیارد");
    num %= 1_000_000_000;
  }

  const million = Math.floor(num / 1_000_000);
  if (million > 0) {
    words.push(numberToPersianWords(million) + " میلیون");
    num %= 1_000_000;
  }

  const thousand = Math.floor(num / 1_000);
  if (thousand > 0) {
    words.push(numberToPersianWords(thousand) + " هزار");
    num %= 1_000;
  }

  const hundred = Math.floor(num / 100);
  if (hundred > 0) {
    words.push(hundreds[hundred - 1]);
    num %= 100;
  }

  const ten = Math.floor(num / 10);
  if (ten > 0) {
    if (ten === 1 && num % 10 > 0) {
      words.push(teens[num % 10]);
    } else {
      words.push(tens[ten]);
    }
    num %= 10;
  }

  if (num > 0) {
    words.push(units[num]);
  }

  // Join the words together and add "تومان" at the end
  return words.filter(Boolean).join(" و ") + " ";
};

const App = () => {
  const [amount, setAmount] = useState("");
  const [amountInWords, setAmountInWords] = useState("");
  const [initialDate, setInitialDate] = useState({
    day: "",
    month: "",
    year: "",
  });
  const [targetDate, setTargetDate] = useState({
    day: "",
    month: "",
    year: "",
  });
  const [modalVisible, setModalVisible] = useState(false);
  const [result, setResult] = useState(null);
  const [errorMessage, setErrorMessage] = useState("");

  const calculateValue = () => {
    setErrorMessage("");
    const {
      day: initialDay,
      month: initialMonth,
      year: initialYear,
    } = initialDate;
    const { day: targetDay, month: targetMonth, year: targetYear } = targetDate;

    if (
      !initialDay ||
      !initialMonth ||
      !initialYear ||
      !targetDay ||
      !targetMonth ||
      !targetYear ||
      !amount ||
      amount <= 0
    ) {
      Alert.alert(
        "Error",
        "لطفا همه فیلدها را پر کنید و مقدار صحیحی وارد کنید."
      );
      return;
    }

    const initial = new Date(initialYear, initialMonth - 1, initialDay);
    const target = new Date(targetYear, targetMonth - 1, targetDay);

    if (initial >= target) {
      setErrorMessage("تاریخ ها بدرستی وارد نشده اند");
      return;
    }

    const timeDifferenceInMonths =
      (target.getFullYear() - initial.getFullYear()) * 12 +
      (target.getMonth() - initial.getMonth());
    const t = timeDifferenceInMonths / 12; // Convert to years

    // Calculate Future Value with an assumed interest rate
    const i = 0.43; // Example interest rate
    const n = 1; // Compounding frequency
    const futureValue = amount * Math.pow(1 + i / n, n * t);

    setResult(
      `ارزش پول شما به تومان در تاریخ ${targetYear}/${targetMonth}/${targetDay} برابر با: ${futureValue.toFixed(
        2
      )} است.`
    );
    setModalVisible(true);
  };

  const handleAmountChange = (input) => {
    const numericValue = parseInt(input.replace(/,/g, ""), 10); // Remove commas
    setAmount(input);

    // Convert numeric input to words in Persian
    if (!isNaN(numericValue) && numericValue >= 0) {
      const wordsRepresentation = numberToPersianWords(numericValue);
      setAmountInWords(wordsRepresentation);
    } else {
      setAmountInWords("");
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>محاسبه ارزش پول</Text>

      <View style={styles.inputGroup}>
        <Text style={styles.amountInWords}>{amountInWords}</Text>
        <TextInput
          style={styles.input}
          placeholder="مقدار پول را وارد کنید"
          keyboardType="numeric"
          value={amount}
          onChangeText={handleAmountChange}
          textAlign="right"
          autoFocus // Automatically focuses the input field, so the cursor blinks
          blurOnSubmit={false} // Keeps the input focused after submit
        />
      </View>

      <View style={styles.datePickerGroup}>
        <Text style={styles.inputLabel}>زمانی که پول دادم یا گرفتم:</Text>
        <View style={styles.datePickers}>
          <Picker
            selectedValue={initialDate.day}
            style={styles.picker}
            onValueChange={(itemValue) =>
              setInitialDate({ ...initialDate, day: itemValue })
            }
          >
            <Picker.Item label="روز" value="" />
            {[...Array(31).keys()].map((i) => (
              <Picker.Item key={i + 1} label={`${i + 1}`} value={`${i + 1}`} />
            ))}
          </Picker>
          <Picker
            selectedValue={initialDate.month}
            style={styles.picker}
            onValueChange={(itemValue) =>
              setInitialDate({ ...initialDate, month: itemValue })
            }
          >
            <Picker.Item label="ماه" value="" />
            {[...Array(12).keys()].map((i) => (
              <Picker.Item key={i + 1} label={`${i + 1}`} value={`${i + 1}`} />
            ))}
          </Picker>
          <Picker
            selectedValue={initialDate.year}
            style={styles.picker}
            onValueChange={(itemValue) =>
              setInitialDate({ ...initialDate, year: itemValue })
            }
          >
            <Picker.Item label="سال" value="" />
            {[...Array(84).keys()].map((i) => (
              <Picker.Item
                key={1403 - i}
                label={`${1403 - i}`}
                value={`${1403 - i}`}
              />
            ))}
          </Picker>
        </View>
      </View>

      <View style={styles.datePickerGroup}>
        <Text style={styles.inputLabel}>ارزش پولم در این تاریخ:</Text>
        <View style={styles.datePickers}>
          <Picker
            selectedValue={targetDate.day}
            style={styles.picker}
            onValueChange={(itemValue) =>
              setTargetDate({ ...targetDate, day: itemValue })
            }
          >
            <Picker.Item label="روز" value="" />
            {[...Array(31).keys()].map((i) => (
              <Picker.Item key={i + 1} label={`${i + 1}`} value={`${i + 1}`} />
            ))}
          </Picker>
          <Picker
            selectedValue={targetDate.month}
            style={styles.picker}
            onValueChange={(itemValue) =>
              setTargetDate({ ...targetDate, month: itemValue })
            }
          >
            <Picker.Item label="ماه" value="" />
            {[...Array(12).keys()].map((i) => (
              <Picker.Item key={i + 1} label={`${i + 1}`} value={`${i + 1}`} />
            ))}
          </Picker>
          <Picker
            selectedValue={targetDate.year}
            style={styles.picker}
            onValueChange={(itemValue) =>
              setTargetDate({ ...targetDate, year: itemValue })
            }
          >
            <Picker.Item label="سال" value="" />
            {[...Array(84).keys()].map((i) => (
              <Picker.Item
                key={1403 - i}
                label={`${1403 - i}`}
                value={`${1403 - i}`}
              />
            ))}
          </Picker>
        </View>
      </View>

      <TouchableOpacity style={styles.calculateButton} onPress={calculateValue}>
        <Text style={styles.buttonText}>محاسبه کن</Text>
      </TouchableOpacity>

      {errorMessage ? (
        <Text style={styles.errorMessage}>{errorMessage}</Text>
      ) : null}

      <Modal
        animationType="slide"
        transparent={true}
        visible={modalVisible}
        onRequestClose={() => setModalVisible(false)}
      >
        <View style={styles.overlay}>
          <View style={styles.popup}>
            <Text style={styles.popupTitle}>نتیجه محاسبه</Text>
            <Text style={styles.popupContent}>{result}</Text>
            <TouchableOpacity onPress={() => setModalVisible(false)}>
              <Text style={styles.popupButton}>بستن</Text>
            </TouchableOpacity>
          </View>
        </View>
      </Modal>

      <View style={styles.footer}>
        <Text>تمام حقوق متعلق به هزار ویترین می باشد@</Text>
        <TouchableOpacity
          onPress={() =>
            Alert.alert(
              "Contact",
              "برای انتقاد یا پیشنهاد جهت بهبود این نرم افزار کلیک کنید"
            )
          }
        >
          <Text style={styles.footerLink}>
            برای انتقاد یا پیشنهاد جهت بهبود این نرم افزار کلیک کنید
          </Text>
        </TouchableOpacity>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#D4EDDA",
    padding: 20,
    justifyContent: "flex-start",
    alignItems: "stretch",
    fontFamily: "Titr",
  },
  title: {
    fontSize: 24,
    textAlign: "center",
    marginBottom: 20,
    color: "black",
    fontFamily: "Titr",
  },
  inputGroup: {
    marginBottom: 15,
    alignItems: "flex-end", // Align items to the end (right).
  },
  amountInWords: {
    fontSize: 16,
    color: "#555",
    fontFamily: "Titr",
    marginBottom: 5,
    textAlign: "right",
  },
  input: {
    height: 40,
    borderColor: "#ccc",
    borderWidth: 1,
    borderRadius: 4,
    padding: 10,
    fontSize: 20,
    fontFamily: "Titr",
    textAlign: "right",
    direction: "rtl",
  },
  datePickerGroup: {
    marginBottom: 20,
  },
  inputLabel: {
    textAlign: "right", // Align label to the right
    fontFamily: "Titr",
    fontSize: 16,
  },
  datePickers: {
    flexDirection: "row-reverse",
    justifyContent: "space-between",
  },
  picker: {
    height: 50,
    width: "30%",
    fontFamily: "Titr",
  },
  calculateButton: {
    backgroundColor: "#28a745",
    paddingVertical: 10,
    borderRadius: 5,
    alignItems: "center",
  },
  buttonText: {
    color: "white",
    fontSize: 18,
    fontFamily: "Titr",
  },
  errorMessage: {
    color: "red",
    textAlign: "center",
    marginTop: 10,
  },
  overlay: {
    flex: 1,
    backgroundColor: "rgba(0, 0, 0, 0.7)", // Dark overlay
    justifyContent: "center",
    alignItems: "center",
  },
  popup: {
    backgroundColor: "#fff",
    width: "80%",
    padding: 20,
    borderRadius: 10,
    elevation: 10,
    shadowColor: "#000",
    shadowOpacity: 0.3,
    shadowRadius: 5,
  },
  popupTitle: {
    fontSize: 20,
    fontWeight: "bold",
    marginBottom: 10,
    color: "#28a745",
    textAlign: "center",
  },
  popupContent: {
    fontSize: 16,
    textAlign: "center",
    marginBottom: 20,
    color: "black",
    fontFamily: "Titr",
  },
  popupButton: {
    backgroundColor: "#28a745",
    color: "white",
    paddingVertical: 10,
    paddingHorizontal: 20,
    borderRadius: 5,
    textAlign: "center",
    fontFamily: "Titr",
  },
  footer: {
    marginTop: "auto",
    paddingVertical: 20,
    alignItems: "center",
    marginBottom: 10,
  },
  footerLink: {
    color: "#28a745",
    fontWeight: "bold",
    textDecorationLine: "underline",
  },
});

export default App;
