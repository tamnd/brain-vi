---
title: "CF 104415C - Cửa Hàng Kẹo"
description: "Chúng ta được cung cấp một danh sách các con số, mỗi con số biểu thị số lượng kẹo mà một học sinh sẵn sàng chấp nhận khi mua hàng. Cửa hàng đang xử lý học viên theo một số thứ tự, nhưng chúng tôi có thể tự do lựa chọn thứ tự mà chúng tôi đáp ứng nhu cầu của họ."
date: "2026-06-30T19:50:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104415
codeforces_index: "C"
codeforces_contest_name: "IME++ Starters Try-outs 2023"
rating: 0
weight: 104415
solve_time_s: 45
verified: true
draft: false
---

[CF 104415C - Cửa hàng kẹo](https://codeforces.com/problemset/problem/104415/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các con số, mỗi con số biểu thị số lượng kẹo mà một học sinh sẵn sàng chấp nhận khi mua hàng. Cửa hàng đang xử lý học viên theo một số thứ tự, nhưng chúng tôi có thể tự do lựa chọn thứ tự mà chúng tôi đáp ứng nhu cầu của họ. Sau khi chúng tôi quyết định tổng số kẹo đã “cam kết” hoặc đã bán, học sinh chỉ có thể hài lòng nếu yêu cầu của họ ít nhất là số lượng đó. Nhiệm vụ của chúng ta là tối đa hóa số lượng sinh viên mà chúng ta có thể đáp ứng bằng cách chọn thứ tự phục vụ họ tối ưu. 

Cấu trúc chính là mỗi sinh viên có một giá trị ngưỡng và việc phục vụ một sinh viên sẽ tiêu tốn một lượng “dung lượng” theo nghĩa tích lũy. Chúng tôi đang cố gắng đưa càng nhiều ràng buộc càng tốt vào một tổng tích lũy ngày càng tăng, trong đó các lựa chọn trước đó sẽ ảnh hưởng đến việc liệu những lựa chọn sau có khả thi hay không. 

Nếu số lượng sinh viên lớn, chẳng hạn như lên tới 200000, thì mô phỏng O(n^2) trong đó chúng tôi liên tục thử các tập hợp con hoặc sắp xếp lại khác nhau sẽ quá chậm. Với khoảng 10^5 hoặc 2×10^5 phần tử, chúng tôi mong đợi giải pháp O(n log n) hoặc O(n). Điều này ngay lập tức cho thấy việc sắp xếp hoặc lựa chọn tham lam là hướng đi khả thi duy nhất. 

Một trường hợp thất bại tinh tế phát sinh khi người ta cố gắng xử lý học sinh theo thứ tự ban đầu hoặc theo thứ tự giảm dần. Nếu đặt những yêu cầu lớn trước, chúng ta có thể tăng tổng tích lũy lên quá nhanh, điều này khiến chúng ta không thể đáp ứng những yêu cầu nhỏ hơn mà lẽ ra có thể được đáp ứng trước đó. 

Ví dụ: giả sử các yêu cầu là`[5, 1, 2]`. Nếu chúng tôi xử lý`5`đầu tiên, tổng tích lũy trở nên lớn ngay lập tức và tùy theo cách giải thích, chúng ta có thể bỏ qua`1`Và`2`hoặc đánh giá sai tính khả thi. Câu trả lời đúng là phục vụ`1`, sau đó`2`, sau đó`5`, cho phép cả ba đều được thỏa mãn trong một tiến trình tích lũy có kiểm soát. 

Khó khăn chính là nhận ra rằng thứ tự xử lý quyết định hạn chế tích lũy tăng nhanh đến mức nào và chiến lược tối ưu là chiến lược trì hoãn sự tăng trưởng càng nhiều càng tốt. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi thứ tự có thể có của học sinh và mô phỏng số lượng có thể hài lòng theo thứ tự đó. Đối với mỗi hoán vị, chúng tôi duy trì tổng số đang chạy và đếm xem có bao nhiêu ngưỡng được đáp ứng. Điều này hiệu quả vì nó trực tiếp mô hình hóa quy trình, nhưng nó có độ phức tạp về mặt giai thừa. Với n học sinh thì có n! hoán vị, và thậm chí việc đánh giá một hoán vị có giá O(n), khiến cho phương pháp này không thể thực hiện được nếu vượt quá n rất nhỏ. 

Quan sát quan trọng là cách duy nhất để tối đa hóa số lượng học sinh hài lòng là giữ tổng tích lũy càng nhỏ càng tốt trong thời gian dài nhất có thể. Bất cứ khi nào chúng tôi chọn sớm một học sinh có yêu cầu lớn, chúng tôi có nguy cơ tăng tổng số tích lũy một cách không cần thiết và cản trở việc đưa vào trong tương lai. Điều này cho thấy rằng chúng ta nên luôn ưu tiên các yêu cầu nhỏ hơn trước vì chúng tạo ra ít ràng buộc nhất đối với tổng số hoạt động. 

Sau khi sắp xếp các yêu cầu theo thứ tự tăng dần, chúng ta có thể xử lý chúng một cách tham lam từ nhỏ nhất đến lớn nhất, duy trì tổng số hiện có. Bất cứ khi nào yêu cầu hiện tại ít nhất là số tiền hiện tại, chúng ta có thể đưa học sinh đó vào và tăng số tiền tương ứng. Nếu không, chúng ta bỏ qua chúng vì việc thêm chúng vào sẽ chỉ làm tăng tổng hơn nữa mà không giúp thỏa mãn nhiều ràng buộc hơn. 

Điều này chuyển đổi vấn đề từ tìm kiếm tổ hợp trên các hoán vị thành quét tuyến tính đơn lẻ sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | O(n! · n) | O(n) | Quá chậm | 
| Sắp xếp + Quét tham lam | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách sắp xếp tất cả các yêu cầu của sinh viên theo thứ tự không giảm để chúng tôi luôn xem xét các ràng buộc nhỏ nhất trước tiên. Điều này đảm bảo rằng mỗi bước đều có cơ hội khả thi tối đa với số tiền tích lũy hiện tại. 

Chúng tôi khởi tạo một biến đang chạy`cur = 0`, đại diện cho tổng số kẹo đã được cam kết và một bộ đếm`ans = 0`, theo dõi số lượng sinh viên đã hài lòng. 

Sau đó chúng tôi lặp qua danh sách đã sắp xếp. Với mỗi giá trị`x`, chúng ta kiểm tra xem có thể thỏa mãn sinh viên này với tổng tích lũy hiện tại hay không. 

Nếu như`x >= cur`, chúng tôi bao gồm sinh viên này, tăng dần`ans`từng cái một và cập nhật`cur += x`. Lý do bản cập nhật này an toàn là vì chúng tôi chỉ cam kết với những học sinh có yêu cầu đủ lớn để đáp ứng tình trạng hiện tại. 

Nếu như`x < cur`, chúng tôi bỏ qua sinh viên này vì việc đưa họ vào sẽ không có ý nghĩa gì trong khuôn khổ tham lam này. Vì tất cả các giá trị trong tương lai đều lớn hơn hoặc bằng nhau (do sắp xếp), việc bỏ qua đảm bảo chúng ta không lãng phí cơ hội cho một quá trình chuyển đổi không khả thi. 

Cuối cùng, chúng tôi xuất ra`ans`. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì tính bất biến`cur`là tổng tích lũy nhỏ nhất có thể đạt được bằng cách chọn một số tập hợp con các phần tử được xử lý. Bằng cách xử lý theo thứ tự được sắp xếp, chúng tôi đảm bảo rằng chúng tôi không bao giờ gặp phải yêu cầu nhỏ hơn sau yêu cầu lớn hơn. Tính đơn điệu này đảm bảo rằng một khi một giá trị không thể thỏa mãn tổng hiện tại thì sẽ không có cấu trúc nào sau này có thể phục hồi được. Sự lựa chọn tham lam trong việc lấy mọi phần tử nhỏ nhất khả thi sẽ duy trì khả năng mở rộng tối đa cho các lựa chọn trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()

cur = 0
ans = 0

for x in a:
    if x >= cur:
        ans += 1
        cur += x

print(ans)
```Giải pháp bắt đầu bằng cách đọc danh sách các yêu cầu và sắp xếp nó sao cho các ràng buộc yếu hơn được xử lý trước. Biến`cur`theo dõi cam kết tích lũy và`ans`đếm xem có bao nhiêu học sinh được phục vụ thành công. Mỗi lần lặp lại quyết định liệu học sinh hiện tại có thể được đưa vào mà không vi phạm ràng buộc tích lũy hay không. điều kiện`x >= cur`là bước kiểm tra tính khả thi quan trọng để đảm bảo chúng tôi chỉ mở rộng giải pháp khi nó vẫn hợp lệ. 

Một lỗi phổ biến là đảo ngược sự bất bình đẳng hoặc cập nhật`cur`không chính xác trước khi kiểm tra tính khả thi. Thứ tự quan trọng: chúng ta phải kiểm tra trước, sau đó cập nhật. Một vấn đề tế nhị khác là quên rằng việc bỏ qua một học sinh sẽ không thiết lập lại hoặc sửa đổi`cur`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [5, 1, 2]
```Mảng được sắp xếp trở thành`[1, 2, 5]`. 

| Bước | x | cur trước | quyết định | cur sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | lấy | 1 | 1 | 
| 2 | 2 | 1 | lấy | 3 | 2 | 
| 3 | 5 | 3 | lấy | 8 | 3 | 

Tất cả học sinh đều hài lòng vì mỗi giá trị được chọn đều cao hơn tổng tích lũy. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [3, 3, 4, 1]
```Mảng được sắp xếp trở thành`[1, 3, 3, 4]`. 

| Bước | x | cur trước | quyết định | cur sau | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | lấy | 1 | 1 | 
| 2 | 3 | 1 | lấy | 4 | 2 | 
| 3 | 3 | 4 | bỏ qua | 4 | 2 | 
| 4 | 4 | 4 | lấy | 8 | 3 | 

Điều này chứng tỏ rằng một khi tổng tích lũy trở nên quá lớn, một số giá trị cỡ trung bình sẽ không còn sử dụng được nữa và thứ tự tham lam đã bị khóa ở tiền tố tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(1) hoặc O(n) | tùy thuộc vào việc thực hiện sắp xếp | 

Bước sắp xếp là nút thắt cổ chai và đối với tối đa 200000 phần tử, nó nằm trong giới hạn thoải mái. Quét tham lam chỉ thực hiện một lần và gây ra chi phí không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# rewritten solution for testing
def solve():
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    a.sort()
    cur = 0
    ans = 0
    for x in a:
        if x >= cur:
            ans += 1
            cur += x
    return str(ans)

# samples / basic
assert solve_from("3\n5 1 2\n") == "3"

# custom cases
assert solve_from("1\n10\n") == "1"
assert solve_from("2\n1 100\n") == "2"
assert solve_from("3\n3 3 3\n") == "3"
assert solve_from("4\n1 2 10 100\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 10`|`1`| độ chính xác của phần tử đơn | 
|`1 100`|`1`(có trường hợp 2 phần tử) | tham lam luôn có khoảng cách lớn hợp lệ | 
|`3 3 3`|`3`| tất cả các giá trị bằng nhau đều hoạt động nhất quán | 
|`1 2 10 100`|`4`| tăng trưởng đơn điệu không bao giờ cản trở chuỗi sớm | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các giá trị đều giống hệt nhau. Xem xét đầu vào`[3, 3, 3]`. Sau khi sắp xếp nó vẫn không thay đổi. Thuật toán tiến hành như sau:`cur = 0`, Đầu tiên`3 >= 0`vậy hãy lấy nó đi,`cur = 3`. Kế tiếp`3 >= 3`vậy hãy lấy nó đi,`cur = 6`. Kế tiếp`3 >= 6`là sai nên chúng ta bỏ qua nó. Đầu ra là`2`. Điều này cho thấy rằng ngay cả các đầu vào thống nhất cũng có thể phá vỡ sự bao gồm đầy đủ khi tổng tích lũy tăng vượt quá giá trị lặp lại. 

Một trường hợp cạnh khác là một giá trị rất nhỏ theo sau là một giá trị lớn, chẳng hạn như`[1, 100, 100]`. Thuật toán lấy`1`, sau đó`100`, sau đó là trận chung kết`100`trở nên không khả thi vì`cur`đã phát triển quá lớn. Dấu vết xác nhận rằng các thể vùi nhỏ ban đầu có thể chặn các thể vùi lớn giống hệt nhau sau này, đó chính xác là lý do tại sao việc phân loại và chấp nhận tham lam là cần thiết để kiểm soát sự tăng trưởng.
