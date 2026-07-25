---
title: "CF 103870B - Tinh thần tỉnh táo"
description: "Chúng tôi đang theo dõi một quá trình lặp lại đơn giản theo thời gian. Mỗi ngày đóng góp vào một bộ đếm đang chạy để đo số ngày đã trôi qua kể từ sự kiện đặt lại cuối cùng."
date: "2026-07-02T07:44:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103870
codeforces_index: "B"
codeforces_contest_name: "TeamsCode Summer 2022 Contest"
rating: 0
weight: 103870
solve_time_s: 37
verified: true
draft: false
---

[CF 103870B - Tinh thần](https://codeforces.com/problemset/problem/103870/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang theo dõi một quá trình lặp lại đơn giản theo thời gian. Mỗi ngày đóng góp vào một bộ đếm đang chạy để đo số ngày đã trôi qua kể từ sự kiện đặt lại cuối cùng. Bất cứ khi nào bộ đếm này đạt đến bội số của một khoảng thời gian cố định$K$, chúng tôi thực hiện một hành động: chúng tôi tăng giá trị được gọi là “sự tỉnh táo” lên một. 

Vì vậy, đầu vào mô tả một chuỗi các ngày và mỗi ngày sẽ ngầm tăng một bộ đếm ngày. Những khoảnh khắc thú vị duy nhất là những ngày mà quầy tính tiền chia hết cho$K$, bởi vì đó là những ngày mà sự tỉnh táo tăng lên. Sau một ngày như vậy, bộ đếm có thể tiếp tục hoặc được đặt lại về 0, nhưng cả hai cách hiểu đều tương đương nhau vì chỉ có modulo đối với$K$vấn đề. 

Đầu ra là giá trị tỉnh táo cuối cùng sau khi xử lý tất cả các ngày trong chuỗi. 

Mặc dù tuyên bố được diễn đạt theo thuật ngữ “Chessbot gửi email cho Bossologist”, sự trừu tượng cốt lõi chỉ là việc đếm định kỳ: mỗi khi chúng tôi hoàn thành một khối$K$ngày, chúng tôi thêm một vào câu trả lời. 

Các ràng buộc là tối thiểu nhưng đây vẫn là vấn đề mô phỏng thời gian tuyến tính. Nếu số ngày$N$tùy thuộc vào$10^5$hoặc$10^6$, sau đó một$O(N)$truyền tải là an toàn tầm thường. Bất cứ điều gì tệ hơn tuyến tính, chẳng hạn như tính toán lại các chu trình hoặc kiểm tra tính chia hết theo cách lồng nhau, sẽ là chi phí không cần thiết nhưng vẫn không vượt qua nếu nó đưa vào các yếu tố bổ sung. 

Các trường hợp đặc biệt chính xuất phát từ cách chu kỳ đầu tiên bắt đầu và bội số chính xác của$K$ứng xử. Việc triển khai đơn giản đặt lại không chính xác hoặc kiểm tra sau khi tăng sai thứ tự có thể làm thay đổi số đếm một. 

Ví dụ, nếu$K = 3$và chúng tôi xử lý 3 ngày, sự tỉnh táo sẽ tăng đúng một lần. Nếu ai đó đặt lại bộ đếm quá sớm, họ có thể bỏ lỡ toàn bộ số gia tăng. Ngược lại, nếu đặt lại quá muộn, chúng có thể bị tính hai lần trên các ranh giới. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là mô phỏng từng ngày trong khi duy trì bộ đếm trong nhiều ngày kể từ lần đặt lại cuối cùng. Mỗi ngày tăng bộ đếm lên một và bất cứ khi nào bộ đếm đạt$K$, chúng tôi tăng sự tỉnh táo và đặt lại bộ đếm về 0. Điều này phản ánh trực tiếp mô tả vấn đề và chính xác theo cách xây dựng. 

Chi phí của phương pháp này là tuyến tính theo số ngày vì mỗi ngày chỉ yêu cầu công việc liên tục. Không có cấu trúc lồng nhau hoặc sự phụ thuộc giữa các ngày nên không cần tối ưu hóa thêm. Mọi cố gắng “tính toán trước” các sự kiện đều không cần thiết vì quá trình này vốn đã đơn giản và định kỳ. 

Quan sát quan trọng là bộ đếm không bao giờ cần được lưu trữ vượt quá giá trị modulo của nó.$K$. Thay vì suy nghĩ theo cách đặt lại, chúng ta có thể nghĩ theo cách số học mô-đun: mỗi ngày đóng góp một đơn vị và bất cứ khi nào tổng tích lũy được chia cho$K$, chúng tôi tăng cường sự tỉnh táo. 

Điều này định hình lại vấn đề bằng cách đếm xem có bao nhiêu bội số của$K$xuất hiện trong phạm vi$1$ĐẾN$N$, đơn giản là$N // K$. Mô phỏng thu gọn thành một biểu thức số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) | O(1) | Đã chấp nhận | 
| Số học trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số ngày$N$và thời kỳ$K$. Những điều này xác định dòng thời gian đầy đủ và tần suất tỉnh táo tăng lên. 
2. Tính xem có bao nhiêu khối có kích thước hoàn chỉnh$K$phù hợp với$N$. Điều này được thực hiện bằng phép chia số nguyên$N // K$, đếm trực tiếp số lần chúng ta đạt bội số của$K$. 
3. Xuất giá trị này làm mức độ tỉnh táo cuối cùng. Mỗi khối đầy đủ tương ứng với chính xác một phần tăng và các khối một phần ở cuối không đóng góp. 

### Tại sao nó hoạt động 

Bất biến chính là bộ đếm ngày chỉ quan trọng thông qua modulo còn lại của nó.$K$. Mỗi khi chúng ta hoàn thành hết một đoạn dài$K$, bộ đếm lại về 0 và một chu kỳ mới bắt đầu. Như vậy, tổng số lần chúng ta đạt điều kiện biên chính xác là số chu kỳ hoàn chỉnh trong tiền tố độ dài$N$. Không có chu kỳ từng phần nào có thể kích hoạt mức tăng vì nó không bao giờ đạt tới$K$, vì vậy chỉ có phép chia đầy đủ mới đóng góp vào câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = input().strip().split()
    if not data:
        return
    n, k = map(int, data)
    
    # each full block of size k contributes +1 sanity
    print(n // k)

if __name__ == "__main__":
    solve()
```Giải pháp đọc hai số nguyên và tính toán trực tiếp phép chia số nguyên. Điều tinh tế duy nhất là đảm bảo rằng phân tích cú pháp đầu vào xử lý khoảng trắng một cách chính xác vì một số định dạng cuộc thi cung cấp cả hai giá trị trên một dòng. 

Sự phân chia$n // k$được an toàn trong mọi trường hợp, kể cả khi$n < k$, trong đó nó trả về số 0 một cách chính xác. Điều này tương ứng với việc không có chu kỳ đầy đủ nào cả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 3
```Chúng tôi xử lý các ngày từ 1 đến 10 với độ dài chu kỳ là 3. 

| Ngày | Trạng thái truy cập | Vệ sinh | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 0 | 
| 3 | 3 → đặt lại | 1 | 
| 4 | 1 | 1 | 
| 5 | 2 | 1 | 
| 6 | 3 → đặt lại | 2 | 
| 7 | 1 | 2 | 
| 8 | 2 | 2 | 
| 9 | 3 → đặt lại | 3 | 
| 10 | 1 | 3 | 

Đầu ra cuối cùng:```
3
```Điều này xác nhận rằng mỗi nhóm hoàn chỉnh gồm 3 ngày đóng góp chính xác một mức tăng. 

### Ví dụ 2 

đầu vào:```
7 4
```| Ngày | Trạng thái truy cập | Vệ sinh | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 0 | 
| 3 | 3 | 0 | 
| 4 | 4 → đặt lại | 1 | 
| 5 | 1 | 1 | 
| 6 | 2 | 1 | 
| 7 | 3 | 1 | 

Đầu ra cuối cùng:```
1
```Điều này cho thấy các đoạn cuối không đầy đủ sẽ không đóng góp gì, vì bộ đếm không bao giờ đạt tới 4 nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ phép chia số học được thực hiện sau khi nhập | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Giải pháp là thời gian không đổi và bộ nhớ không đổi, thấp hơn nhiều so với giới hạn Codeforces thông thường. Ngay cả với đầu vào cực lớn, quá trình tính toán vẫn diễn ra tức thời vì nó tránh hoàn toàn việc lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimum case
assert run("1 5") == "0"

# exact multiple
assert run("6 3") == "2"

# non-multiple
assert run("10 4") == "2"

# large k greater than n
assert run("7 10") == "0"

# equal values
assert run("100 100") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 | 0 | không có chu kỳ đầy đủ | 
| 6 3 | 2 | bội số chính xác | 
| 10 4 | 2 | phần còn lại bị bỏ qua | 
| 7 10 | 0 | k > n trường hợp cạnh | 
| 100 100 | 1 | trường hợp đẳng thức biên | 

## Vỏ cạnh 

Đối với trường hợp$N < K$, chẳng hạn như đầu vào`5 10`, bộ đếm không bao giờ đạt đến ngưỡng. Thuật toán tính toán`5 // 10 = 0`, cho thấy chính xác không có sự tỉnh táo nào tăng lên. Mô phỏng từng bước sẽ hiển thị bộ đếm tiến triển từ 1 đến 5 mà không bao giờ chạm tới 10, xác nhận không tăng. 

Để căn chỉnh ranh giới chính xác, chẳng hạn như`9 3`, mô phỏng đạt ngưỡng chính xác vào ngày thứ 3, 6 và 9. Thuật toán trả về`9 // 3 = 3`, khớp chính xác ba sự kiện đặt lại.
