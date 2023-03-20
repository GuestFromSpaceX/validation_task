## validation_task
# Выполнение валидации вводимого номера банковской карты и ее эмитента. (Ермолинский)
###### *Мой старший разработчик сказал, что тут нет хардкода, так что я довольный его сюда добавляю.*

Гиф кода. 
![Screencast_from_20 03 2023_12_13_02](https://user-images.githubusercontent.com/100123572/226296811-3a792054-0093-4ebc-be39-d70a0f21ff6e.gif)


PHP код.

```php
<?php

$cardclass = new Validate();
$cardclass->cardNumber = readline("ВЕДИТЕ НОМЕР СВОЕЙ КАРТЫ ДЛЯ ОПРЕДЕЛЕНИЯ ВАЛИДНОСТИ КАРТЫ И ЕЕ ЭМИТЕНТА:" . "\n");
$cardclass->validateCardNumber($cardclass->cardNumber);
$cardclass->validateCardIssuer($cardclass->cardNumber);

class Validate
{
    public $cardNumber;

    public function validateCardNumber($cardNumber)
    {
        $validationCardNumberSum = 0;

        if (strlen($cardNumber) == 16) {
            for ($i = 0; $i < 16; $i++) {
                if (($i % 2 == 0) and ($cardNumber[$i]*2 > 9)) {
                    $validationCardNumberSum += ($cardNumber[$i]*2) - 9;
                } elseif (($i % 2 == 0) and ($cardNumber[$i]*2 < 9)) {
                    $validationCardNumberSum += $cardNumber[$i]*2;
                } else {
                    $validationCardNumberSum += $cardNumber[$i];
                }
            }
            if (($validationCardNumberSum >= 40) and
                ($validationCardNumberSum <= 140) and
                ($validationCardNumberSum % 10 == 0)) {
                print_r( "********************" . "\n");
                print_r("Valid card number! *" . "\n");
                print_r($cardNumber . "   *\n");
                print_r("********************" . "\n");
                return 1;
            }   else {
                print_r("********************" . "\n");
                print_r("Invalid card number*" . "\n");
                print_r("Please try again   *" . "\n");
                print_r($cardNumber . "   *\n");
                print_r("********************" . "\n");
                return 0;
            }

        }   else {
            print_r("**********************" . "\n");
            print_r("Incorrect card number*" . "\n");
            print_r("Please try again     *" . "\n");
            print_r($cardNumber . str_repeat(" ", 21 - strlen($cardNumber)) . "*\n");
            print_r("**********************" . "\n");
            return 0;
        }

    }

    public function validateCardIssuer($cardNumber)
    {
        $mir = preg_match('/\b(2)([0-9]+)\b/', $cardNumber);
        $amex = preg_match('/\b(34|37)([0-9]+)\b/', $cardNumber);
        $jcb = preg_match('/\b(31|35)([0-9]+)\b/', $cardNumber);
        $dc = preg_match('/\b(30|36|38)([0-9]+)\b/', $cardNumber);
        $visa = preg_match('/\b(4)([0-9]+)\b/', $cardNumber);
        $master = preg_match('/\b(51|52|53|54|55)([0-9]+)\b/', $cardNumber);
        $maestro = preg_match('/\b(50|56|57|58|63|67)([0-9]+)\b/', $cardNumber);
        $china = preg_match('/\b(62)([0-9]+)\b/', $cardNumber);
        $discover = preg_match('/\b(60)([0-9]+)\b/', $cardNumber);
        $uec = preg_match('/\b(7)([0-9]+)\b/', $cardNumber);

        $cartName = [
            "Mir",
            "American Express",
            "JCB International",
            "Diners Club",
            "VISA",
            "MasterCard",
            "Maestro",
            "China UnionPay",
            "Discover",
            "UEK"
        ];

        $binaryCartsValue = [
            $mir,
            $amex,
            $jcb,
            $dc,
            $visa,
            $master,
            $maestro,
            $china,
            $discover,
            $uec
        ];

        if ($binaryCartsValue == [0, 0, 0, 0, 0, 0, 0, 0, 0, 0] or strlen($cardNumber) != 16) {
            print_r("Your Card Issuer is NOT defind" . "\n");
            print_r(str_repeat("#", strlen("NOT DEFIND")+2) . "\n");
            print_r("#" . "NOT_DEFIND" . "#" . "\n");
            print_r(str_repeat("#", strlen("NOT DEFIND")+2) . "\n");
            return 0;
        } else {
            for ($i = 0; $i < 10; $i++) {
                if ($binaryCartsValue[$i] == 1) {
                    print_r("Your Card Issuer is" . "\n");
                    print_r(str_repeat("#", strlen($cartName[$i]) + 2) . "\n");
                    print_r("#" . $cartName[$i] . "#" . "\n");
                    print_r(str_repeat("#", strlen($cartName[$i]) + 2) . "\n");
                    return 1;
                } else continue;
            }
        }
    }
}
```
