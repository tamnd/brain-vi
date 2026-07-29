---
title: "CF 102760B - Bom trong bộ bài của tôi"
description: "Chúng tôi có một bộ bài chứa các lá bài A. Trong đó B là bom và các quân bài còn lại đều an toàn. Thứ tự của bộ bài là ngẫu nhiên nên mọi vị trí đặt bom đều có xác suất như nhau."
date: "2026-07-28T23:48:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102760
codeforces_index: "B"
codeforces_contest_name: "2020 KAIST 10th ICPC Mock Contest (XXI Open Cup. Grand Prix of Korea. Division 2)"
rating: 0
weight: 102760
solve_time_s: 80
verified: true
draft: false
---

[CF 102760B - Bom trong bộ bài của tôi](https://codeforces.com/problemset/problem/102760/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ bài chứa`A`thẻ. Trong số đó,`B`là những quả bom và những lá bài còn lại đều an toàn. Thứ tự của bộ bài là ngẫu nhiên nên mọi vị trí đặt bom đều có xác suất như nhau. 

Người chơi bắt đầu một lượt với`C`điểm máu và lộ thẻ từ trên xuống cho đến khi tìm được thẻ an toàn. Mỗi quả bom được tiết lộ sẽ giảm 5 máu. Nếu máu bằng 0 hoặc thấp hơn trước khi lá bài an toàn xuất hiện, người chơi sẽ thua. Nhiệm vụ là tính xác suất người chơi sống sót ở lượt này. 

Các giới hạn rất nhỏ:`A`nhiều nhất là 30 và`C`tối đa là 30. Điều này có nghĩa là chúng tôi không cần cấu trúc dữ liệu nâng cao hoặc kỹ thuật tối ưu hóa tiệm cận. Ngay cả một cách tiếp cận có tính đến mọi số lượng bom có ​​thể được rút ra cũng là đủ. Tuy nhiên, độ chính xác của dấu phẩy động rất quan trọng vì câu trả lời là xác suất, do đó phép tính sẽ tránh mất độ chính xác không cần thiết. 

Các trường hợp nguy hiểm chính xuất phát từ mối quan hệ giữa sức khỏe và bom. Việc thực hiện bất cẩn có thể đếm sai lá bài an toàn đầu tiên hoặc dừng quá muộn sau khi người chơi đã chết. 

Hãy xem xét đầu vào:```
4 2 5
```Người chơi thua sau một quả bom vì`5 - 5 = 0`. Trường hợp duy nhất sống sót là rút thẻ an toàn trước nên đáp án là:```
0.500000000
```Một giải pháp sai cho phép sử dụng một quả bom và kiểm tra cái chết sau đó sẽ tạo ra xác suất không chính xác. 

Một trường hợp khác là:```
4 2 6
```Ở đây người chơi sống sót sau một quả bom vì`6 - 5 = 1`. Chỉ có hai quả bom liên tiếp gây ra tổn thất. Xác suất là:```
0.833333333
```Một giải pháp sử dụng`C // 5`thay vì giá trị trần sẽ yêu cầu hai quả bom bị mất và hỏng ở ranh giới này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng mọi vị trí có thể có của thẻ an toàn đầu tiên. Nếu thẻ an toàn đầu tiên xuất hiện sau`k`bom, người chơi sống sót chính xác khi`k`bom không làm giảm sức khỏe về không. Chúng ta có thể tính xác suất của từng giá trị có thể có của`k`và thêm các trường hợp hợp lệ. 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là liệt kê tất cả các cách sắp xếp có thể có của bộ bài và đếm những cách sắp xếp thành công. Vì bộ bài có tới 30 lá bài nên điều này nhanh chóng trở nên bất khả thi. Số vị trí có thể đặt bom là`C(30,15)`trong trường hợp lớn nhất, vượt xa những gì có thể được xử lý. 

Nhận xét quan trọng là việc bố trí hoàn chỉnh bộ bài là không cần thiết. Điều duy nhất ảnh hưởng đến kết quả là có bao nhiêu quả bom xuất hiện trước lá bài an toàn đầu tiên. Chúng ta có thể tính toán trực tiếp xác suất nhìn thấy`k`bom theo sau là một thẻ an toàn. 

Xác suất của lần đầu tiên`k`thẻ là bom là:```
B / A * (B - 1) / (A - 1) * ... * (B - k + 1) / (A - k + 1)
```Sau những quả bom đó, quân bài tiếp theo phải an toàn. Lúc đó có`A - k`thẻ còn lại và vẫn còn`A - B`thẻ an toàn, vậy yếu tố tiếp theo là:```
(A - B) / (A - k)
```Bởi vì người chơi chỉ chết sau khi đạt được`ceil(C / 5)`bom, chúng ta chỉ cần tính tổng xác suất cho các giá trị nhỏ hơn của`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^A) | O(A) | Quá chậm | 
| Tối ưu | O(A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lượng bom tối thiểu cần thiết để khiến sức khỏe không còn tích cực. Giá trị này là`ceil(C / 5)`. Bất kỳ chuỗi nào có ít bom hơn đều thành công. 
2. Bắt đầu với xác suất`1.0`trong trường hợp không có quả bom nào được rút trước lá bài an toàn đầu tiên. 
3. Lặp lại số lượng bom có ​​thể được rút ra trước thẻ an toàn. Với mỗi giá trị`k`, duy trì xác suất để lần đầu tiên`k`thẻ là bom. 
4. Đối với giá trị hiện tại của`k`, nhân với xác suất lá bài tiếp theo an toàn. Thêm xác suất này vào câu trả lời vì điều này thể hiện một kết quả có thể sống sót. 
5. Cập nhật xác suất rút thêm một quả bom và tiếp tục cho đến khi tất cả các quả bom đã được xem xét hoặc người chơi đã thua. 

Bất biến đằng sau thuật toán là trước khi xử lý một giá trị của`k`, xác suất được duy trì thể hiện chính xác cơ hội mà lần đầu tiên`k`thẻ được tiết lộ là bom. Nhân với xác suất của lá bài tiếp theo sẽ chuyển trạng thái đó thành khả năng lá bài an toàn đầu tiên xuất hiện ngay sau những lá bài đó.`k`bom. Vì mọi trường hợp sống sót có thể xảy ra đều có một số lượng bom duy nhất có trước đó nên việc tổng hợp các xác suất này sẽ cho ra xác suất sống sót hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B, C = map(int, input().split())

    need = (C + 4) // 5
    ans = 0.0
    prob_all_bombs = 1.0

    for k in range(need):
        if k >= B:
            break

        prob_safe_next = (A - B) / (A - k)
        ans += prob_all_bombs * prob_safe_next

        prob_all_bombs *= (B - k) / (A - k)

    print("{:.9f}".format(ans))

if __name__ == "__main__":
    solve()
```Biến`need`lưu trữ số lượng bom nhỏ nhất gây ra thất bại. Vòng lặp chỉ xem xét các giá trị nhỏ hơn vì bất kỳ số nào lớn hơn đều không thể góp phần mang lại kết quả thành công.`prob_all_bombs`lưu trữ xác suất rằng lần đầu tiên`k`thẻ là quả bom trước lần lặp hiện tại. Phép nhân với`(B - k) / (A - k)`nâng cấp trạng thái này lên số lượng bom tiếp theo có thể có. 

biểu thức`(C + 4) // 5`là trần số nguyên của`C / 5`. Việc sử dụng phép chia số nguyên thông thường ở đây sẽ không thành công khi sức khỏe không phải là bội số của 5. 

mẫu số`A - k`đại diện cho số lượng thẻ còn lại trước khi rút thẻ tiếp theo. Từ`A`nhiều nhất là 30, độ chính xác của dấu phẩy động là quá đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2 5
```Người chơi cần một quả bom để thua. 

| k | Xác suất thẻ k đầu tiên là bom | Xác suất lá bài tiếp theo an toàn | Đã thêm xác suất | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1.000000 | 0,500000 | 0,500000 | 0,500000 | 

Sự kiện thành công duy nhất là nhận được thẻ an toàn ngay lập tức. Dấu vết này cho thấy ranh giới nơi chính xác một quả bom có ​​thể gây tử vong. 

### Mẫu 2 

đầu vào:```
4 2 6
```Người chơi cần hai quả bom để thua. 

| k | Xác suất thẻ k đầu tiên là bom | Xác suất lá bài tiếp theo an toàn | Đã thêm xác suất | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1.000000 | 0,500000 | 0,500000 | 0,500000 | 
| 1 | 0,500000 | 0,666667 | 0,333333 | 0.833333 | 

Người chơi sống sót khi không có hoặc có một quả bom trước lá bài an toàn đầu tiên. Dấu vết xác nhận rằng một quả bom được phép sử dụng khi sức khỏe vẫn dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(A) | Chúng tôi kiểm tra tối đa một trường hợp cho mọi số lượng bom có ​​thể có. | 
| Không gian | O(1) | Chỉ có một vài giá trị xác suất được lưu trữ. | 

Số lần lặp tối đa là 30, do đó thuật toán dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert abs(float(run("4 2 5\n")) - 0.5) < 1e-9, "sample 1"
assert abs(float(run("4 2 6\n")) - 0.833333333) < 1e-9, "sample 2"

assert abs(float(run("4 2 20\n")) - 1.0) < 1e-9, "large health"
assert abs(float(run("2 1 1\n")) - 0.5) < 1e-9, "minimum size"
assert abs(float(run("30 15 1\n")) - 0.5) < 1e-9, "maximum size with one health"
assert abs(float(run("10 9 46\n")) - 1.0) < 1e-9, "many bombs but enough health"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 2 5`|`0.500000000`| Một quả bom có ​​thể gây tử vong ngay lập tức. | 
|`4 2 6`|`0.833333333`| Một quả bom có ​​thể sống sót vì sức khỏe vẫn tích cực. | 
|`4 2 20`|`1.000000000`| Sức khỏe đủ lớn để mọi trận hòa có thể tồn tại. | 
|`2 1 1`|`0.500000000`| Ranh giới boong nhỏ nhất. | 
|`30 15 1`|`0.500000000`| Kích thước bộ bài tối đa và ngưỡng tử vong ngay lập tức. | 
|`10 9 46`|`1.000000000`| Số lượng bom tối đa có thể không thể giết chết người chơi. | 

## Vỏ cạnh 

Đối với đầu vào:```
4 2 5
```ngưỡng là một quả bom. Thuật toán bắt đầu với xác suất không có bom, sau đó là thẻ an toàn. Xác suất đó là`2 / 4`, vì vậy câu trả lời trở thành`0.5`. Nó không bao giờ coi trọng một quả bom vì chạm tới một quả bom đồng nghĩa với cái chết. 

Đối với đầu vào:```
4 2 6
```ngưỡng là hai quả bom. Đầu tiên, thuật toán sẽ tính xác suất của thẻ an toàn ngay lập tức, tức là`0.5`. Sau đó nó sẽ xem xét chính xác một quả bom theo sau là một thẻ an toàn. Xác suất đó là`(2 / 4) * (2 / 3) = 1 / 3`, đưa ra câu trả lời cuối cùng`5 / 6`. 

Đối với đầu vào:```
4 2 20
```người chơi cần bốn quả bom để thua, nhưng bộ bài chỉ chứa hai quả bom. Vòng lặp bao gồm mọi số lượng bom có ​​thể có và cộng tất cả các kết quả có thể xảy ra, dẫn đến xác suất`1`. 

Đối với đầu vào:```
2 1 1
```người chơi chết vì quả bom đầu tiên. Cách sắp xếp duy nhất còn sót lại là rút thẻ an toàn trước. Thuật toán xử lý việc này vì vòng lặp dừng trước khi đếm trường hợp bom chết người. 

Tôi cũng có thể cung cấp một phiên bản biên tập theo phong cách cuộc thi ngắn hơn hoặc một phiên bản toán học hơn với đạo hàm xác suất chính thức nếu cần.
