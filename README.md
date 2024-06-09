import java.util.Scanner;

class Calc {

    public static void main(String[] args) throws Exception {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Введите два числа (арабских или римских): ");
        System.out.println("Пример -> 5+5 ");
        System.out.println("Пример -> X+IX ");
        String expression = scanner.nextLine();
        System.out.println(parse(expression));
    }

    public static String parse(String expression) throws Exception {
        int num1;
        int num2;
        String oper;
        String result;
        boolean isRimsk;
        String[] operands = expression.split("[+\\-*/]");
        if (operands.length != 2) throw new Exception("Должно быть два числа");
        oper = detectOperation(expression);
        if (oper == null) throw new Exception("Нет такой операции");


        if (Rimsk.isRimsk(operands[0]) && Rimsk.isRimsk(operands[1])) {

            num1 = Rimsk.convertToArabian(operands[0]);
            num2 = Rimsk.convertToArabian(operands[1]);

            if (num1 > 10) {
                throw new IllegalArgumentException("Первое число должно быть не больше 10-ти!");
            }
            if (num2 > 10) {
                throw new IllegalArgumentException("Второе число должно быть не больше 10-ти!");
            }
            isRimsk = true;
        }

        else if (!Rimsk.isRimsk(operands[0]) && !Rimsk.isRimsk(operands[1])) {
            num1 = Integer.parseInt(operands[0]);
            num2 = Integer.parseInt(operands[1]);
            if (num1 > 10) {
                throw new IllegalArgumentException("Первое число должно быть не больше 10-ти!");
            }
            if (num2 > 10) {
                throw new IllegalArgumentException("Второе число должно быть не больше 10-ти!");
            }
            isRimsk = false;
        }

        else {
            throw new Exception("Числа должны быть в одном формате");
        }
        int arabian = calc(num1, num2, oper);
        if (isRimsk) {

            if (arabian <= 0) {
                throw new Exception("Римское число должно быть больше нуля");
            }

            result = Rimsk.convertToRimsk(arabian);
        } else {

            result = String.valueOf(arabian);
        }

        return result;
    }

    static String detectOperation(String expression) {
        if (expression.contains("+")) return "+";
        else if (expression.contains("-")) return "-";
        else if (expression.contains("*")) return "*";
        else if (expression.contains("/")) return "/";
        else return null;
    }

    static int calc(int a, int b, String oper) {
        return switch (oper) {
            case "+" -> a + b;
            case "-" -> a - b;
            case "*" -> a * b;
            default -> a / b;
        };
    }
}

class Rimsk {
    static String[] RimskArray = new String[]{ "НоЛь", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX", "X", "XI",
            "XII", "XIII", "XIV", "XV", "XVI", "XVII", "XVIII", "XIX", "XX", "XXI", "XXII", "XXIII", "XXIV",
            "XXV", "XXVI", "XXVII", "XXVIII", "XXIX", "XXX", "XXXI", "XXXII", "XXXIII", "XXXIV", "XXXV", "XXXVI",
            "XXXVII", "XXXVIII", "XXXIX", "XL", "XLI", "XLII", "XLIII", "XLIV", "XLV", "XLVI", "XLVII", "XLVIII",
            "XLIX", "L", "LI", "LII", "LIII", "LIV", "LV", "LVI", "LVII", "LVIII", "LIX", "LX", "LXI", "LXII",
            "LXIII", "LXIV", "LXV", "LXVI", "LXVII", "LXVIII", "LXIX", "LXX", "LXXI", "LXXII", "LXXIII", "LXXIV",
            "LXXV", "LXXVI", "LXXVII", "LXXVIII", "LXXIX", "LXXX", "LXXXI", "LXXXII", "LXXXIII", "LXXXIV", "LXXXV",
            "LXXXVI", "LXXXVII", "LXXXVIII", "LXXXIX", "XC", "XCI", "XCII", "XCIII", "XCIV", "XCV", "XCVI", "XCVII",
            "XCVIII", "XCIX", "C"};

    public static boolean isRimsk(String val) {
        for (String s : RimskArray) {
            if (val.equals(s)) {
                return true;
            }
        }
        return false;
    }

    public static int convertToArabian(String Rimsk) {
        for (int i = 0; i < RimskArray.length; i++) {
            if (Rimsk.equals(RimskArray[i])) {
                return i;
            }
        }
        return -1;
    }

    public static String convertToRimsk(int arabian) {
        return RimskArray[arabian];
    }
}
