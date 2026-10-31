---
title: "CF 106282A - \u0421\u043e\u0432\u0443\u043d\u044c\u044f \u0438 \u0441\u043e\u043b\u0435\u043d\u0438\u044f"
description: "Bài toán mô tả một tình huống đơn giản: có n cây nấm cần được xếp đều vào k lọ. Nếu n không chia hết cho k thì phải thu thêm nấm."
date: "2026-06-25T07:39:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106282
codeforces_index: "A"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 (\u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435) 9 \u043a\u043b\u0430\u0441\u0441, \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c, 2025"
rating: 0
weight: 106282
solve_time_s: 28
verified: true
draft: false
---

[CF 106282A - \u0421\u043e\u0432\u0443\u043d\u044c\u044f \u0438 \u0441\u043e\u043b\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/106282/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Vấn đề mô tả một tình huống đơn giản: có`n`nấm cần được đặt đều vào`k`lọ. Nếu như`n`không chia hết cho`k`, phải thu thập thêm vài cây nấm nữa. Mục tiêu là tìm ra số lượng nấm bổ sung nhỏ nhất để tổng số nấm có thể chia đều cho tất cả các lọ. 

Đầu vào chứa hai số nguyên. Đầu tiên là số lượng nấm hiện tại, thứ hai là số lọ. Đầu ra là số lượng nấm bổ sung tối thiểu cần thiết. 

Các ràng buộc cho phép cả hai giá trị đều lớn bằng`10^9`. Điều đó ngay lập tức loại trừ việc cố gắng mô phỏng việc thêm từng cây nấm một, vì trong trường hợp xấu nhất, bản thân câu trả lời có thể gần bằng`10^9`. Ngay cả một vòng lặp có hàng trăm triệu lần lặp cũng sẽ quá chậm. Giải pháp cần sử dụng số học theo thời gian không đổi. 

Các trường hợp đặc biệt chính đến từ việc xử lý các số đã chia hết và diễn giải chính xác phần còn lại. 

Coi như`n = 8`Và`k = 4`. Đầu ra đúng là`0`, vì tám cây nấm đã đổ đầy bốn lọ như nhau. Một giải pháp bất cẩn luôn luôn bổ sung`k - n % k`vẫn hoạt động ở đây, nhưng nếu triển khai không đúng với trường hợp đặc biệt bị thiếu, nó có thể trả về`4`. 

Một trường hợp khác là số lượng nấm ít hơn số lọ. Ví dụ:```
n = 3
k = 5
```Đầu ra đúng là`2`, vì sau khi thu thập thêm hai cây nấm nữa sẽ có năm cây nấm, mỗi lọ một cây. Một giải pháp chỉ kiểm tra xem`n / k`đã dương có thể kết luận không chính xác rằng không thể phân phối được. 

Trường hợp biên cuối cùng là khi phần dư gần bằng`k`. Ví dụ:```
n = 17
k = 10
```Đầu ra đúng là`3`, bởi vì`17 + 3 = 20`, là bội số đầu tiên của mười sau mười bảy. Việc quên rằng chúng ta cần phải lấy bội số tiếp theo chứ không chỉ loại bỏ phần dư, dẫn đến câu trả lời sai. 

# Phương pháp tiếp cận 

Cách tiếp cận đơn giản là cộng liên tục một cây nấm cho đến khi tổng số chia hết cho`k`. Điều này có tác dụng vì mỗi câu trả lời hợp lệ phải có một số phép cộng nào đó di chuyển`n`đến bội số của`k`. Nếu chúng ta cố gắng`0`, sau đó`1`, sau đó`2`, v.v., giá trị thành công đầu tiên được đảm bảo ở mức tối thiểu. 

Vấn đề là câu trả lời có thể rất lớn. Giả định`n = 1`Và`k = 10^9`. Một mô phỏng sẽ yêu cầu gần một tỷ lần lặp trước khi đạt được tổng số hợp lệ. Điều đó vượt xa những gì một chương trình có thể làm trong thời gian giới hạn của cuộc thi thông thường. 

Quan sát quan trọng là khả năng chia hết chỉ phụ thuộc vào số dư sau khi chia cho`k`. Nếu như`n % k = r`, sau đó`n`đã rồi`r`nấm ra khỏi bội số trước đó của`k`. Để đạt được bội số tiếp theo, chúng ta cần thêm`k - r`nấm. Ngoại lệ duy nhất là khi`r = 0`, vì số này đã hợp lệ và chúng ta không cần thêm gì nữa. 

Toàn bộ vấn đề giảm xuống còn việc tính toán phần còn lại và chuyển đổi nó thành khoảng cách sang bội số tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) trong trường hợp xấu nhất | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc số lượng nấm hiện tại`n`và số lọ`k`. 
2. Tính toán`n % k`, cho chúng ta biết còn lại bao nhiêu nấm sau khi tạo ra bao nhiêu nhóm hoàn chỉnh`k`càng tốt. 
3. Nếu số dư bằng 0, xuất ra`0`vì nấm đã có thể được phân phối đồng đều. 
4. Ngược lại, xuất ra`k - (n % k)`, bởi vì đó chính xác là số lượng nấm cần thiết để di chuyển từ phần còn lại hiện tại sang bội số tiếp theo của`k`. 

Tại sao nó hoạt động: mọi số chia hết cho`k`là bội số của`k`. Bắt đầu từ`n`, bội số lớn nhất gần nhất của`k`thu được bằng cách tăng số dư lên`k`. Nếu phần còn lại đã bằng 0 thì bội số hợp lệ gần nhất chính là số đó. Vì chúng ta di chuyển đến bội số gần nhất có thể nên số lượng nấm được thêm vào là tối thiểu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
k = int(input())

rem = n % k

if rem == 0:
    print(0)
else:
    print(k - rem)
```Hai dòng đầu tiên đọc hai giá trị từ đầu vào. Không có nhiều trường hợp thử nghiệm nên chương trình xử lý trực tiếp những con số này. 

Việc tính toán phần còn lại là cốt lõi của giải pháp. Số nguyên Python có thể xử lý các giá trị lớn hơn nhiều so với`10^9`, do đó không có mối quan tâm tràn. 

các`rem == 0`điều kiện là cần thiết vì`k - rem`sẽ cho`k`trong tình huống đó, nhưng câu trả lời đúng là bằng không. Đây là trường hợp đặc biệt duy nhất cần thiết. 

# Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
4
```| Bước | n | k | n % k | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đọc giá trị | 3 | 4 | - | - | 
| Tính số dư | 3 | 4 | 3 | - | 
| Thêm nấm vào nhiều tiếp theo | 3 | 4 | 3 | 1 | 

Ba cây nấm còn lại ba cây khi được nhóm vào lọ cỡ bốn. Thêm một cây nấm nữa sẽ tạo ra bốn cây nấm, mỗi cây có thể đặt một cây vào mỗi lọ. 

### Ví dụ 2 

đầu vào:```
7
5
```| Bước | n | k | n % k | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đọc giá trị | 7 | 5 | - | - | 
| Tính số dư | 7 | 5 | 2 | - | 
| Thêm nấm vào nhiều tiếp theo | 7 | 5 | 2 | 3 | 

Bảy cây nấm để lại hai cây còn lại sau khi loại bỏ nhóm năm cây. Thêm ba cây nấm sẽ tạo ra mười cây nấm, mỗi lọ có thể chia thành hai cây nấm. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một phép chia và một vài phép tính số học được thực hiện. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên. | 

Các ràng buộc cho phép các giá trị lên tới`10^9`và thuật toán không phụ thuộc vào kích thước của câu trả lời. Nó dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

# Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    n = int(input())
    k = int(input())

    rem = n % k
    ans = 0 if rem == 0 else k - rem

    sys.stdin = old_stdin
    return str(ans) + "\n"

# provided samples
assert solve("3\n4\n") == "1\n", "sample 1"
assert solve("7\n5\n") == "3\n", "sample 2"

# custom cases
assert solve("8\n4\n") == "0\n", "already divisible"
assert solve("1\n1000000000\n") == "999999999\n", "large gap"
assert solve("1000000000\n1000000000\n") == "0\n", "maximum equal values"
assert solve("17\n10\n") == "3\n", "next multiple boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`8 / 4`|`0`| Xử lý các số đã chia hết cho số jar | 
|`1 / 1000000000`|`999999999`| Xử lý số lượng bổ sung lớn nhất có thể | 
|`1000000000 / 1000000000`|`0`| Kiểm tra giá trị tối đa và phân chia chính xác | 
|`17 / 10`|`3`| Kiểm tra việc di chuyển đến bội số tiếp theo một cách chính xác | 

# Vỏ cạnh 

cho`n = 8`Và`k = 4`, thuật toán tính toán`8 % 4 = 0`. Vì số dư bằng 0 nên nó ngay lập tức trả về`0`. Không cần thêm nấm vì việc phân phối đã có thể thực hiện được. 

Vì`n = 3`Và`k = 5`, thuật toán tính toán`3 % 5 = 3`. Bội số tiếp theo của năm là năm, do đó nó tính toán`5 - 3 = 2`. Sau khi thêm hai cây nấm thì có năm cây nấm và mỗi lọ nhận được một cây. 

Vì`n = 17`Và`k = 10`, thuật toán tính toán`17 % 10 = 7`. Bội số tiếp theo của mười là hai mươi, tức là cách ba. câu trả lời`3`xác nhận rằng thuật toán luôn tiến tới tổng hợp lệ gần nhất thay vì chỉ sử dụng phần còn lại hiện tại.
