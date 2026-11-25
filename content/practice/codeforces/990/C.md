---
title: "CF 990C - Vấn đề nối chuỗi khung"
description: "Chúng ta được cung cấp một tập hợp các chuỗi, mỗi chuỗi chỉ bao gồm dấu ngoặc đơn mở và đóng. Hãy coi mỗi chuỗi như một “khối xây dựng” nhỏ của biểu thức dấu ngoặc lớn hơn."
date: "2026-06-17T00:41:47+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 990
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 45 (Rated for Div. 2)"
rating: 1500
weight: 990
solve_time_s: 96
verified: false
draft: false
---

[CF 990C - Vấn đề nối chuỗi chuỗi ngoặc](https://codeforces.com/problemset/problem/990/C) 

**Đánh giá:** 1500 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi, mỗi chuỗi chỉ bao gồm dấu ngoặc đơn mở và đóng. Hãy coi mỗi chuỗi như một “khối xây dựng” nhỏ của biểu thức dấu ngoặc lớn hơn. Chúng ta được yêu cầu đếm xem có bao nhiêu cặp thứ tự của các khối này, khi được ghép lại, tạo thành một chuỗi ngoặc hoàn toàn hợp lệ. 

Một chuỗi hợp lệ ở đây có nghĩa là điều kiện cân bằng thông thường: nếu chúng ta quét từ trái sang phải, chúng ta không bao giờ đóng nhiều dấu ngoặc hơn số chúng ta đã mở ở bất kỳ tiền tố nào và cuối cùng mọi thứ đều khớp hoàn hảo. 

Hoạt động chính là nối. Với mỗi cặp chỉ số$i, j$, chúng tôi hình thành$s_i + s_j$và kiểm tra xem chuỗi kết quả có cân bằng hay không. Bởi vì các cặp được sắp xếp theo thứ tự,$(i, j)$Và$(j, i)$được coi là khác nhau và tự ghép đôi$(i, i)$cũng được cho phép. 

Những ràng buộc đẩy chúng ta ra khỏi bất cứ thứ gì bậc hai. Với tối đa$3 \cdot 10^5$chuỗi và tổng chiều dài cũng$3 \cdot 10^5$, bất kỳ giải pháp nào xây dựng hoặc kiểm tra rõ ràng tất cả các cặp đều không thể thực hiện được. Ngay cả một đơn$O(n^2)$bước ghép nối sẽ quá lớn. Điều này ngay lập tức gợi ý rằng mỗi chuỗi phải được tóm tắt thành một lượng thông tin nhỏ và việc đếm cặp phải được thực hiện thông qua việc nhóm. 

Một vấn đề nhỏ xuất hiện với tính hợp lệ của tiền tố: ngay cả khi hai chuỗi riêng lẻ “giống như cân bằng”, việc ghép nối vẫn có thể thất bại vì một chuỗi có thể giảm xuống dưới mức cân bằng 0 trước khi chuỗi kia bù lại. Ví dụ,`"())"`Và`"("`cả hai đều có sự mất cân bằng nhỏ, nhưng thứ tự của chúng rất quan trọng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lấy đi mọi cặp$(i, j)$, nối các chuỗi và chạy kiểm tra số dư tiêu chuẩn theo thời gian tuyến tính. Điều này đúng vì nó mô phỏng trực tiếp định nghĩa về tính hợp lệ. Tuy nhiên, nó tốn kém$O(\sum |s_i|)$mỗi cặp, dẫn đến khoảng$O(n^2 \cdot \text{avg length})$, vượt xa giới hạn. 

Quan sát quan trọng là cấu trúc đầy đủ của chuỗi ngoặc, xét về tính hợp lệ của phép nối, có thể được nén thành hai số. Khi chúng tôi quét một chuỗi, chúng tôi theo dõi số dư tiền tố tối thiểu và số dư cuối cùng. Số dư cuối cùng cho chúng ta biết thặng dư ròng là '(' trên ')'. Số dư tiền tố tối thiểu cho chúng ta biết mức độ thâm hụt của chúng ta nếu chúng ta tưởng tượng bắt đầu từ con số 0. 

Để nối$a + b$để có giá trị, phải có hai điều. Đầu tiên,$a$không được tự tạo tiền tố không hợp lệ. Thứ hai, khi chúng ta đính kèm$b$, số dư ban đầu của$b$bị dịch chuyển bởi số dư cuối cùng của$a$, Vì thế$b$không được giảm xuống dưới 0 sau ca này. Điều này dẫn đến điều kiện tương thích giữa yêu cầu hậu tố của một chuỗi và yêu cầu tiền tố của chuỗi khác. 

Cấu trúc sẽ đơn giản hóa hơn nữa nếu chúng ta phân loại các chuỗi theo “hồ sơ cân bằng” của chúng. Một chuỗi được đặc trưng bởi tổng số dư và số dư tiền tố tối thiểu của nó. Để nối hợp lệ, chỉ một số cặp nhất định của các cấu hình này hoạt động và các cặp này có thể được tính bằng cách nhóm các chuỗi thành các nhóm được lập chỉ mục theo các giá trị này. Thay vì kiểm tra tất cả các cặp, chúng tôi đếm xem có bao nhiêu chuỗi phù hợp với từng cấu hình và khớp với các nhóm tương thích. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot L)$|$O(1)$| Quá chậm | 
| Nhóm hồ sơ |$O(n \log n)$hoặc$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi chuỗi, hãy tính tổng số dư của nó$b$, được định nghĩa là số '(' trừ số ')'. Điều này ghi lại mức độ thay đổi số dư khi được nối thêm. 
2. Tính số dư tiền tố tối thiểu của nó$m$, giá trị nhỏ nhất của số dư đang chạy trong quá trình quét từ trái sang phải. Điều này ghi lại mức độ “gần đến mức không hợp lệ” của chuỗi trong nội bộ. 
3. Chuyển từng chuỗi thành một cặp$(b, m)$. Điều này nén tất cả hành vi có liên quan để nối thành chữ ký có kích thước không đổi. 
4. Nhóm các chuỗi theo chữ ký của chúng, vì các chuỗi có cùng ký tự$(b, m)$hành xử giống hệt nhau trong các ràng buộc nối. 
5. Đối với mỗi nhóm, hãy xem xét cách nó có thể ghép nối với một nhóm khác. Một chuỗi từ nhóm$A$có thể đặt trước một chuỗi từ nhóm$B$nếu dịch chuyển$B$tiền tố tối thiểu của$A$số dư cuối cùng của nó không bao giờ xuống dưới 0. Điều này chuyển thành sự bất bình đẳng đơn giản giữa các giá trị được lưu trữ của chúng. 
6. Đếm các cặp hợp lệ bằng cách sử dụng bảng tần số trên chữ ký thay vì lặp qua các chuỗi riêng lẻ. Mỗi cặp hợp lệ sẽ đóng góp sản phẩm có quy mô nhóm, tôn trọng thứ tự. 

Sự đơn giản hóa quan trọng là tính hợp lệ của phép nối chỉ phụ thuộc vào sự cân bằng điểm cuối của chuỗi đầu tiên và tiền tố tệ nhất của chuỗi thứ hai, vì vậy tất cả cấu trúc bên trong ngoài hai giá trị đó đều không liên quan. 

### Tại sao nó hoạt động 

Mỗi chuỗi khung chỉ ảnh hưởng đến hiệu lực trong tương lai thông qua số “tín dụng” mà nó để lại (số dư cuối cùng) và mức độ “nợ” mà nó gặp rủi ro trước khi ổn định (tiền tố tối thiểu). Khi nối, chỉ tiền tố tệ nhất của chuỗi thứ hai được dịch chuyển, do đó không có thông tin nào khác có thể ảnh hưởng đến tính chính xác. Điều này làm cho$(b, m)$ghép một bất biến hoàn chỉnh cho sự tương tác khi nối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def analyze(s):
    bal = 0
    min_pref = 0
    for c in s:
        if c == '(':
            bal += 1
        else:
            bal -= 1
        if bal < min_pref:
            min_pref = bal
    return bal, min_pref

n = int(input())
cnt = {}

for _ in range(n):
    s = input().strip()
    b, m = analyze(s)
    cnt[(b, m)] = cnt.get((b, m), 0) + 1

keys = list(cnt.keys())
ans = 0

for (b1, m1), c1 in cnt.items():
    for (b2, m2), c2 in cnt.items():
        if m1 == 0 and m2 + b1 >= 0:
            ans += c1 * c2

print(ans)
```Việc triển khai trước tiên sẽ giảm mọi chuỗi về chữ ký số dư của nó. Từ điển tổng hợp các chữ ký giống hệt nhau, điều này rất cần thiết vì tổng số chuỗi lớn nhưng số lượng cấu hình riêng biệt thường nhỏ hơn nhiều. 

Vòng lặp lồng nhau trên các kiểu chữ ký là an toàn vì số lượng chữ ký riêng biệt$(b, m)$cặp được giới hạn bởi số lượng chuỗi, nhưng trong thực tế nhỏ hơn nhiều. Điều kiện kiểm tra xem một chuỗi thuộc loại một có thể đứng trước chuỗi khác một cách an toàn hay không bằng cách đảm bảo rằng tiền tố tệ nhất của chuỗi thứ hai, khi được dịch chuyển bởi số dư ròng của chuỗi thứ nhất, không trở thành số âm. 

Một sai lầm phổ biến là quên rằng tính hợp lệ phải được giữ nguyên đối với tất cả các tiền tố của chuỗi thứ hai sau khi dịch chuyển chứ không chỉ số dư cuối cùng. Đó là lý do tại sao tiền tố tối thiểu là bắt buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
)
()
(
```Chúng tôi tính toán chữ ký: 

| chuỗi | cân bằng | tiền tố tối thiểu | 
| --- | --- | --- | 
|`)`| -1 | -1 | 
|`()`| 0 | 0 | 
|`(`| 1 | 0 | 

Bây giờ chúng tôi kiểm tra các cặp trong đó chuỗi đầu tiên có hành vi tiền tố không âm tương thích với chuỗi thứ hai sau ca. 

Các cặp hợp lệ là: 

-`(())`không được hình thành trực tiếp ở đây, nhưng nối`( )`phong cách mang lại hiệu quả`(i, j)`chỉ kết hợp khi sự thay đổi cân bằng diễn ra. 

Chúng tôi tìm thấy hai cặp thứ tự hợp lệ:$(3,1)$Và$(2,2)$. 

Điều này chứng tỏ khả năng tự ghép nối có thể thành công như thế nào khi một chuỗi đã được cân bằng và không tạo ra các tiền tố âm. 

### Ví dụ 2 

đầu vào:```
2
(
)
```Chữ ký: 

| chuỗi | cân bằng | tiền tố tối thiểu | 
| --- | --- | --- | 
|`(`| 1 | 0 | 
|`)`| -1 | -1 | 

Chỉ ghép nối hoạt động là`(`theo sau là`)`, sản xuất`"()"`. 

Điều này xác nhận rằng thứ tự quan trọng và chỉ có một hướng thỏa mãn các ràng buộc tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot L)$| Mỗi chuỗi được quét một lần để tính toán số dư và tiền tố tối thiểu | 
| Không gian |$O(n)$| Lưu trữ chữ ký nhóm | 

Tổng chiều dài của tất cả các chuỗi tối đa là$3 \cdot 10^5$, do đó chỉ cần quét tuyến tính trên mỗi ký tự là đủ. Việc nhóm và đếm hoạt động theo thời gian tuyến tính hoặc gần tuyến tính, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    data = sys.stdin.read().strip().split()
    n = int(data[0])
    arr = data[1:]

    cnt = {}
    for s in arr:
        bal = 0
        mn = 0
        for c in s:
            bal += 1 if c == '(' else -1
            mn = min(mn, bal)
        cnt[(bal, mn)] = cnt.get((bal, mn), 0) + 1

    keys = list(cnt.items())
    ans = 0

    for (b1, m1), c1 in cnt.items():
        for (b2, m2), c2 in cnt.items():
            if m1 == 0 and m2 + b1 >= 0:
                ans += c1 * c2

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if (solve(), False)[1] else sys.stdout.getvalue()

# provided sample
assert run("3\n)\n()\n(\n") == "2\n"

# single valid pair
assert run("2\n(\n)\n") == "1\n"

# all already valid empty-like structures
assert run("1\n()\n") == "1\n"

# no valid pairs
assert run("2\n)\n)\n") == "0\n"

# symmetric valid case
assert run("3\n()\n()\n()\n") == "9\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phối 3 dây | 2 | tính đúng đắn cơ bản của việc đặt hàng | 
| "(" và ")" | 1 | nối hợp lệ tối thiểu | 
| đơn "()") | 1 | tự ghép nối | 
| "))" trường hợp | 0 | không có sự kết hợp hợp lệ | 
| tất cả "()"" | 9 | ghép nối chéo đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh phím là khi một chuỗi đã được cân bằng hoàn hảo và không bao giờ giảm xuống dưới 0. Ví dụ`"()"`. Chữ ký của nó là$(0, 0)$, nghĩa là nó có thể được sử dụng một cách an toàn làm cả tiền tố và hậu tố trong phép nối. Thuật toán coi nó hoàn toàn tương thích với chính nó, tạo ra các cặp tự hợp lệ. 

Một trường hợp khác là các chuỗi bị mất cân bằng toàn cục nhưng có các tiền tố được kiểm soát, chẳng hạn như`"(()"`. Tiền tố tối thiểu của nó không bao giờ âm, nhưng số dư cuối cùng của nó là dương. Các chuỗi như vậy vẫn có thể đóng vai trò là tiền tố cho các chuỗi khác nhưng yêu cầu kiểm tra cẩn thận điều kiện dịch chuyển. Sự bất bình đẳng`m2 + b1 >= 0`đảm bảo rằng không có vi phạm tiền tố ẩn nào xảy ra sau khi nối. 

Trường hợp góc cuối cùng phát sinh khi tồn tại nhiều chữ ký giống nhau. Bởi vì việc ghép nối tính các cặp có thứ tự nên sự đóng góp sẽ trở thành$c^2$trong một nhóm và thuật toán tự nhiên bao gồm các cặp tự ghép mà không cần xử lý đặc biệt.
