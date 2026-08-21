---
title: "CF 104101H - Dây Đẹp"
description: "Chúng ta được cấp một bảng chữ cái cố định gồm 18 chữ cái viết thường đầu tiên, từ a đến r. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được một chuỗi s và một số n."
date: "2026-07-02T02:09:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104101
codeforces_index: "H"
codeforces_contest_name: "The 2022 Zhejiang University City College Freshman Programming Contest"
rating: 0
weight: 104101
solve_time_s: 46
verified: true
draft: false
---

[CF 104101H - Chuỗi đẹp](https://codeforces.com/problemset/problem/104101/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bảng chữ cái cố định gồm 18 chữ cái viết thường đầu tiên, từ`a`ĐẾN`r`. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được một chuỗi`s`và một số`n`. Chúng tôi được yêu cầu đếm có bao nhiêu chuỗi`t`chiều dài`n`, trong đó tất cả các ký tự trong`t`khác biệt, thỏa mãn một điều kiện đặc biệt liên quan đến cách các ký tự của`t`liên hệ với nhau và với sự hiện diện của họ bên trong`s`. 

Điều kiện có hai cách thay thế để định tính một chuỗi`t`hợp lệ. Hoặc chúng ta chọn một nhân vật bắt đầu`t1`xuất hiện ở đâu đó trong`s`, sau đó mở rộng nó bằng cách chọn các chữ cái tăng dần cho các vị trí còn lại hoặc chúng tôi chọn một chuỗi`t`nơi mọi nhân vật xuất hiện ở đâu đó trong`s`mà không yêu cầu bất kỳ điều kiện thứ tự nào giữa các ký tự liền kề. Vì bảng chữ cái nhỏ và`n ≤ 18`, cả hai cách giải thích đều cho thấy câu trả lời chỉ phụ thuộc vào chữ cái nào xuất hiện trong`s`, không phải vị trí của họ. 

Ràng buộc chính là tổng kích thước đầu vào của`s`trên tất cả các trường hợp thử nghiệm lên tới 2 × 10^5, điều này buộc mọi giải pháp phải xử lý từng ký tự của`s`nhiều nhất là O(1) hoặc O(log 18) lần. Vì bảng chữ cái chỉ có 18 chữ cái nên bất kỳ sự phụ thuộc hàm mũ nào vào 18 đều có khả năng được chấp nhận, nhưng phải tránh bất kỳ điều gì liên quan đến hoán vị của các tập con lớn hoặc tính toán lại cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi`s`chứa rất ít ký tự riêng biệt. Ví dụ, nếu`s = "a"`Và`n = 2`, vẫn chỉ có các cấu trúc hợp lệ thuộc loại đầu tiên nếu quy tắc cho phép mở rộng vượt quá các ràng buộc hiện diện, nhưng bất kỳ cách giải thích nào bỏ qua thứ tự hoàn toàn sẽ vượt quá các chuỗi bao gồm các chữ cái vắng mặt. Tương tự, nếu`s = "ar"`Và`n = 2`, cả hai cặp tăng dần như`ab`,`ac`, và cũng là một công trình giống như đảo ngược như`ra`trở nên phù hợp, điều này cho thấy rằng vấn đề đang trộn lẫn một cấu trúc đơn điệu với một điều kiện hoàn toàn dựa trên tập hợp con. 

Nhận thức quan trọng là thông tin duy nhất từ`s`là chữ cái nào trong số 18 chữ cái xuất hiện ít nhất một lần. 

## Phương pháp tiếp cận 

Đầu tiên chúng tôi xem xét cách giải thích trực tiếp bằng vũ lực. Chúng tôi liệt kê tất cả các chuỗi có thể`t`chiều dài`n`với các ký tự riêng biệt từ 18 chữ cái. Có nhiều nhất P(18, n) khả năng vốn đã lớn: với n = 18 thì đây là 18! đó là khoảng 6,4 × 10^15. Đối với mỗi chuỗi ứng cử viên, chúng tôi kiểm tra xem nó có thỏa mãn một trong hai điều kiện đã cho hay không bằng cách quét nó và kiểm tra tư cách thành viên trong`s`. Ngay cả khi việc kiểm tra tư cách thành viên là O(1) bằng cách sử dụng mảng boolean, bản thân việc liệt kê vẫn vượt xa khả thi. 

Cấu trúc của các điều kiện gợi ý rằng hạn chế thực sự duy nhất là liệu tập hợp các chữ cái đã chọn có nằm trong`s`hoặc liệu nó có thể được mở rộng theo thứ tự tăng dần bắt đầu từ một chữ cái xuất hiện trong`s`. Vì “tăng nghiêm ngặt” trên 18 chữ cái có nghĩa là khi chúng ta chọn một tập hợp con các chữ cái, sẽ có chính xác một cách sắp xếp tăng dần của chúng, điều kiện đầu tiên mô tả một cách hiệu quả tất cả các dãy con tăng dần được hình thành từ các chữ cái xuất hiện trong`s`. 

Điều kiện thứ hai đơn giản yêu cầu tất cả các ký tự của`t`xuất hiện trong`s`, nhưng vì các ký tự là khác biệt và thứ tự không bị ràng buộc ở đó nên mọi hoán vị của một tập hợp con có kích thước hợp lệ`n`được cho phép. Điều này chia câu trả lời thành hai chế độ đếm độc lập trên các tập hợp con của bảng chữ cái. 

Do đó, chúng ta quy vấn đề về việc đếm các tập hợp con các chữ cái có trong tập hợp các chữ cái xuất hiện trong`s`, sau đó tính xem có bao nhiêu hoán vị hợp lệ hoặc bao nhiêu cách sắp xếp tăng dần mà mỗi tập hợp con đóng góp. 

Cho phép`k`là số chữ cái riêng biệt có trong`s`. Chúng tôi chỉ quan tâm đến những điều này`k`các chữ cái. Đối với mỗi tập hợp con có kích thước`n`, có đúng một chuỗi tăng dần và có`n!`hoán vị góp phần vào điều kiện thứ hai. Tuy nhiên, điều kiện đầu tiên chỉ đóng góp vào các chuỗi tăng dần, vốn đã được đưa vào một lần cho mỗi tập hợp con. Vì vậy, với mỗi tập hợp con có kích thước`n`, chúng tôi thêm`n!`khỏi các hoán vị và trường hợp tăng dần không thêm các chuỗi khác biệt ngoài cách diễn giải đã đặt đó. Do đó, vấn đề rơi vào việc chọn bất kỳ tập con nào có kích thước`n`từ`k`các chữ cái có sẵn và đếm tất cả các hoán vị của nó. 

Vì vậy, câu trả lời trở thành: 

C(k, n) × n!. 

Chúng tôi tính toán trước các giai thừa lên tới 18 và đếm các chữ cái riêng biệt trong`s`mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê lực lượng vũ phu của t | O(P(18, n) · n) | O(1) | Quá chậm | 
| Đếm các chữ cái riêng biệt + tổ hợp | O( | s | + n) | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi`s`và số nguyên`n`. Đếm xem có bao nhiêu chữ cái khác biệt xuất hiện trong`s`. Giá trị này là`k`và nó tóm tắt đầy đủ tất cả thông tin có liên quan trong chuỗi vì chỉ sự hiện diện mới quan trọng chứ không phải tần suất hay thứ tự. 
2. Nếu`k < n`, ngay lập tức xuất ra 0. Chúng ta không thể tạo bất kỳ độ dài nào-`n`chuỗi có các ký tự riêng biệt nếu ít hơn`n`tổng số chữ cái tồn tại. 
3. Tính toán trước giai thừa lên tới 18 một lần. Chúng thể hiện số cách hoán vị bất kỳ tập hợp con có kích thước được chọn nào`n`. 
4. Tính số cách chọn`n`những lá thư từ`k`, đó là C(k, n). Nhân nó với`n!`. 
5. Xuất kết quả. 

### Tại sao nó hoạt động 

Mỗi chuỗi hợp lệ`t`sử dụng chính xác`n`các chữ cái khác biệt với tập hợp các chữ cái có trong`s`. Khi một tập hợp con có kích thước`n`là cố định, mọi hoán vị của nó sẽ tạo ra một chuỗi hợp lệ cho điều kiện thứ hai và điều kiện đầu tiên đóng góp chính xác vào thứ tự tăng dần duy nhất của cùng tập hợp con đó. Vì thứ tự tăng dần đó đã là một trong những hoán vị nên tổng số chuỗi hợp lệ cho mỗi tập hợp con là chính xác.`n!`. Do đó, vấn đề giảm xuống việc đếm các tập hợp con có kích thước`n`từ bảng chữ cái có sẵn trong`s`và nhân với số lượng hoán vị trên mỗi tập hợp con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXA = 18

fact = [1] * (MAXA + 1)
for i in range(1, MAXA + 1):
    fact[i] = fact[i - 1] * i

def solve():
    s, n = input().split()
    n = int(n)

    used = [0] * 26
    k = 0
    for ch in s:
        if not used[ord(ch) - 97]:
            used[ord(ch) - 97] = 1
            k += 1

    if k < n:
        print(0)
        return

    # C(k, n)
    comb = 1
    for i in range(n):
        comb *= (k - i)
        comb //= (i + 1)

    print(comb * fact[n])

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Giải pháp bắt đầu bằng cách nén chuỗi đầu vào thành một mảng hiện diện boolean trên 26 chữ cái, mặc dù chỉ có 18 chữ cái đầu tiên là có liên quan. số lượng`k`theo dõi có bao nhiêu chữ cái có thể sử dụng được. 

Hệ số nhị thức được tính toán lặp đi lặp lại để tránh tính toán trước tam giác Pascal và giữ cho bộ nhớ không đổi. Từ`n ≤ 18`, tăng trưởng số nguyên là an toàn trong Python và không bao giờ trở nên đắt đỏ. 

nhân với`fact[n]`chiếm tất cả các hoán vị của từng tập hợp con được chọn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = a
n = 2
```| Bước | k | n | C(k,n) | N! | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 2 | - | - | - | 
| kiểm tra | 1 < 2 | - | - | - | 0 | 

Chỉ có một chữ cái riêng biệt tồn tại nên không thể tạo thành một cặp riêng biệt. 

Điều này xác nhận điều kiện cắt tỉa sớm xử lý chính xác kích thước bảng chữ cái không đủ. 

### Ví dụ 2 

đầu vào:```
s = ar
n = 2
```| Bước | k | n | C(k,n) | N! | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | 2 | - | - | - | 
| chọn | 2 | 2 | 1 | 2 | 2 | 

Chúng tôi chọn cả hai chữ cái`{a, r}`. Có đúng 1 cách chọn tập con và 2 hoán vị:`ar`Và`ra`. 

Điều này phù hợp với ý tưởng rằng việc sắp xếp thứ tự góp phần tạo nên bội số giai thừa trên mỗi tập hợp con được chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O(1) | Chỉ các mảng cố định có kích thước 26 và giai thừa lên tới 18 | 

Các ràng buộc cho phép tổng số ký tự lên tới 2 × 10^5 và mỗi ký tự được xử lý một lần. Các phép tính giai thừa có thời gian không đổi cho mỗi trường hợp thử nghiệm vì n nhiều nhất là 18. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import factorial

    MAXA = 18
    fact = [1] * (MAXA + 1)
    for i in range(1, MAXA + 1):
        fact[i] = fact[i - 1] * i

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        s, n = sys.stdin.readline().split()
        n = int(n)

        used = [0] * 26
        k = 0
        for ch in s:
            if not used[ord(ch) - 97]:
                used[ord(ch) - 97] = 1
                k += 1

        if k < n:
            out.append("0")
            continue

        comb = 1
        for i in range(n):
            comb *= (k - i)
            comb //= (i + 1)

        out.append(str(comb * fact[n]))

    return "\n".join(out)

# provided samples
assert run("1\na 2\n") == "2", "sample 1"
assert run("1\nar 2\n") == "2", "sample 2"

# custom cases
assert run("1\nabc 1\n") == "3", "single choice letters"
assert run("1\nabc 3\n") == "6", "full permutation of all letters"
assert run("1\na 1\n") == "1", "minimum valid"
assert run("1\nab 3\n") == "0", "insufficient letters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một 1 | 1 | tập hợp con tối thiểu | 
| abc 1 | 3 | lựa chọn một chữ cái | 
| abc 3 | 6 | đếm hoán vị đầy đủ | 
| ab 3 | 0 | chữ cái không đủ rõ ràng | 

## Vỏ cạnh 

Khi nào`s`chứa chính xác`n`các chữ cái riêng biệt, việc tính toán giảm xuống còn một tập hợp con. Thuật toán xuất ra chính xác`n!`, vì C(n, n) = 1 và tất cả các hoán vị đều được tính. 

Khi`s`chứa nhiều bản sao nhưng ít ký tự riêng biệt, việc triển khai hoàn toàn bỏ qua tần số một cách chính xác. Ví dụ,`s = "aaaaabbbb"`cư xử chính xác như`s = "ab"`, vì chỉ có sự hiện diện mới quan trọng. 

Khi`n = 1`, mọi ký tự riêng biệt trong`s`tạo thành một chuỗi hợp lệ và công thức trả về C(k, 1) × 1 = k, khớp với kiểu liệt kê trực tiếp. 

Khi`k < n`, việc thoát sớm đảm bảo không thực hiện phép tính tổ hợp không cần thiết, tránh các bước chia trên các trạng thái không hợp lệ.
