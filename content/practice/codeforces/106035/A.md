---
title: "CF 106035A - Đường dẫn kim tự tháp"
description: "Chúng ta được cho một chuỗi biểu diễn một chuyến đi thăm mỗi đỉnh đúng một lần, do đó nó đã là một hoán vị của các số từ 1 đến n. Nhiệm vụ là xác định xem thứ tự này có hình dạng rất cụ thể hay không. Hình dạng được phép là "kim tự tháp" theo nghĩa sau."
date: "2026-06-25T12:55:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106035
codeforces_index: "A"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2024"
rating: 0
weight: 106035
solve_time_s: 49
verified: true
draft: false
---

[CF 106035A - Đường dẫn kim tự tháp](https://codeforces.com/problemset/problem/106035/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi biểu diễn một chuyến đi thăm mỗi đỉnh đúng một lần, do đó nó đã là một hoán vị của các số từ 1 đến n. Nhiệm vụ là xác định xem thứ tự này có hình dạng rất cụ thể hay không. 

Hình dạng được phép là "kim tự tháp" theo nghĩa sau. Nếu bạn nhìn vào hoán vị từ trái sang phải, trước tiên nó phải di chuyển theo một hướng về mặt giá trị (tăng hoặc giảm nghiêm ngặt), sau đó nó phải chuyển hướng đúng một lần và tiếp tục theo hướng ngược lại. Cả hai phần phải có độ dài ít nhất là hai, mặc dù bản thân bước ngoặt thuộc về cả hai cách diễn giải đơn điệu theo nghĩa là nó được chia sẻ bởi hai đoạn. 

Vì vậy, cấu trúc về cơ bản là một đỉnh hoặc một đáy duy nhất: giá trị tăng rồi giảm hoặc giảm rồi tăng, không cho phép đảo ngược lần thứ hai. 

Đầu vào chỉ là n theo sau là hoán vị. Đầu ra đơn giản là có hoặc không tùy thuộc vào việc cấu trúc đơn điệu thay đổi đơn lẻ đó có tồn tại hay không. 

Ràng buộc n lên tới 100000 buộc bất kỳ giải pháp nào cũng phải tuyến tính, vì mọi giải pháp bậc hai sẽ có khoảng 10^10 phép tính trong trường hợp xấu nhất và ngay lập tức không thể thực hiện được trong giới hạn thời gian thông thường. Ngay cả cách tiếp cận O(n log n) cũng có thể chấp nhận được nhưng không cần thiết ở đây; cấu trúc đủ cục bộ nên chỉ cần một lượt là đủ. 

Các trường hợp đặc biệt chính là những thất bại tinh vi của các chuỗi “gần như đơn điệu”. 

Ví dụ đầu tiên là một mảng hoàn toàn đơn điệu như [1, 2, 3, 4, 5]. Thoạt nhìn nó có vẻ chấp nhận được vì nó không có vi phạm, nhưng nó không bao giờ tạo thành hai đoạn có sự thay đổi hướng nên phải bị từ chối. 

Một ví dụ khác là [4, 5, 6, 1, 2, 3]. Điều này có một mức tăng rõ ràng, sau đó là sự giảm về mặt giá trị thực tế, nhưng quá trình chuyển đổi không nhất quán với một đỉnh hoặc đáy duy nhất trên toàn bộ chuỗi vì hướng thay đổi nhiều lần khi diễn giải theo thứ tự hoán vị đầy đủ. 

Một trường hợp tinh tế hơn là dao động ngắn như [1, 3, 2]. Điều này hợp lệ vì nó tăng một lần rồi giảm một lần và cả hai đoạn đều có đủ độ dài. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ thử mọi điểm phân chia có thể có i, coi tiền tố là một hướng đơn điệu và hậu tố là hướng ngược lại. Đối với mỗi lần phân chia, chúng tôi sẽ xác minh tính đơn điệu bằng cách quét cả hai nửa. Điều này dẫn đến hành vi O(n^2) vì mỗi lần chia đều tốn O(n) và có các lần chia O(n). Với n = 10^5, điều này vượt xa giới hạn khả thi. 

Cấu trúc của bài toán là dãy đã là một hoán vị nên việc so sánh giữa các dãy lân cận sẽ mô tả đầy đủ mọi thay đổi về xu hướng. Thay vì kiểm tra tất cả các điểm phân chia, chúng ta chỉ cần phát hiện xem hướng của các hiệu liền kề thay đổi bao nhiêu lần. 

Nếu chúng ta tính dấu của từng hiệu liền kề, toàn bộ vấn đề sẽ giảm xuống còn việc kiểm tra xem chuỗi dấu có nhiều nhất một sự chuyển đổi từ dương sang âm hay âm sang dương hay không và đoạn ban đầu có ít nhất một bước theo mỗi hướng nếu có sự thay đổi. Điều này tương đương với việc đếm số lần chuỗi chuyển đổi giữa xu hướng tăng và giảm. 

Điểm mấu chốt là bất kỳ chuỗi hình chóp nào cũng chỉ có thể có một cực trị, do đó hướng thay đổi nhiều nhất một lần. Nếu nó thay đổi 0 lần thì chuỗi sẽ đơn điệu và không hợp lệ. Nếu nó thay đổi một lần thì nó hợp lệ miễn là cả hai bên đều không trống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra phân chia Brute Force | O(n^2) | O(1) | Quá chậm | 
| Đếm thay đổi hướng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý những khác biệt liền kề và theo dõi những thay đổi về hướng.

1. Tính xem mỗi cặp liền kề tăng hay giảm bằng cách so sánh a[i] và a[i+1]. Điều này nén chuỗi thành một biểu diễn xu hướng nhị phân. 
2. Đi qua mảng xu hướng này và đếm số lần xu hướng chuyển từ tăng sang giảm hoặc ngược lại. Mỗi lần lật tương ứng với một đỉnh hoặc một đáy tiềm năng. 
3. Nếu số lần lật lớn hơn 1, cấu trúc không thể có hình chóp vì nó hàm ý có nhiều bước ngoặt. 
4. Nếu không có lần lật nào, mảng hoàn toàn đơn điệu và không chứa cấu trúc đỉnh hoặc đáy đơn cần thiết nên không hợp lệ. 
5. Nếu có chính xác một lần lật, hãy kiểm tra xem cả hai đoạn được tạo bởi lần lật này có độ dài ít nhất là 2 trong chuỗi ban đầu. Điều này đảm bảo “kim tự tháp” có các phần tăng dần và giảm dần có ý nghĩa. 

Ý tưởng cốt lõi là chúng tôi đang giảm điều kiện hình dạng tổng thể thành thuộc tính chuyển tiếp cục bộ. 

### Tại sao nó hoạt động 

Điều bất biến là dãy luôn được hiểu là một đường dẫn đơn điệu từng đoạn. Mỗi khi nhận thấy sự thay đổi dấu hiệu của các khác biệt liền kề, chúng ta xác định được ranh giới giữa các đoạn đơn điệu. Một đường dẫn hình chóp hợp lệ phải có chính xác hai đoạn đơn điệu: một đoạn tăng và một đoạn giảm, theo một trong hai thứ tự. Nếu có nhiều hơn một ranh giới tồn tại thì có ít nhất ba đoạn đơn điệu, nghĩa là chuỗi thay đổi hướng nhiều lần, vi phạm định nghĩa. Nếu không có ranh giới tồn tại, chuỗi không bao giờ đổi hướng nên nó không thể tạo thành kim tự tháp. Việc duy trì tính bất biến này qua một lần duy nhất đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

# build trend array: +1 for up, -1 for down
trend = []
for i in range(n - 1):
    if a[i] < a[i + 1]:
        trend.append(1)
    else:
        trend.append(-1)

changes = 0
for i in range(1, len(trend)):
    if trend[i] != trend[i - 1]:
        changes += 1

# valid pyramidal path must have exactly one change
if changes != 1:
    print("NO")
else:
    print("YES")
```Đầu tiên, mã nén hoán vị thành một chuỗi hướng dẫn giữa các lân cận. Điều này tránh việc lý luận trực tiếp về các giá trị mà thay vào đó tập trung vào những thay đổi về cấu trúc. 

Vòng thứ hai đếm số lần hướng này bị đảo ngược. Số lượng đó tương ứng trực tiếp với số lượng phân đoạn đơn điệu trừ đi một. 

Cuối cùng, quyết định kiểm tra xem có chính xác một lần lật như vậy hay không, đó là dấu hiệu của cấu trúc đỉnh hoặc đáy duy nhất. 

Một điểm tinh tế là sự đẳng thức không bao giờ xảy ra vì đầu vào là một hoán vị, do đó mọi so sánh liền kề đều tăng hoặc giảm nghiêm ngặt. Điều đó loại bỏ sự mơ hồ trong việc trích xuất xu hướng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
4 3 1 2 5
```Xây dựng xu hướng: 

| tôi | a[i] so với a[i+1] | xu hướng | 
| --- | --- | --- | 
| 0 | 4 > 3 | -1 | 
| 1 | 3 > 1 | -1 | 
| 2 | 1 < 2 | +1 | 
| 3 | 2 < 5 | +1 | 

Phát hiện thay đổi: 

| tôi | xu hướng[i-1] | xu hướng[i] | thay đổi | 
| --- | --- | --- | --- | 
| 1 | -1 | -1 | 0 | 
| 2 | -1 | +1 | 1 | 
| 3 | +1 | +1 | 1 | 

Tổng số thay đổi = 1, vì vậy đầu ra là CÓ. 

Điều này khẳng định một cấu trúc thung lũng duy nhất: giảm rồi tăng. 

### Ví dụ 2 

đầu vào:```
6
4 5 6 1 2 3
```Xu hướng: 

| tôi | xu hướng | 
| --- | --- | 
| 0 | +1 | 
| 1 | +1 | 
| 2 | -1 | 
| 3 | +1 | 
| 4 | +1 | 

Phát hiện thay đổi: 

| tôi | thay đổi | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Tổng số thay đổi = 2, vì vậy đầu ra là KHÔNG. 

Điều này thể hiện một chuỗi có nhiều hướng dịch chuyển, nghĩa là nó có nhiều hơn một cực trị và không thể có dạng hình chóp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét hoán vị một lần để xây dựng xu hướng và một lần để đếm các thay đổi | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng xu hướng có kích thước n−1 | 

Các ràng buộc cho phép tối đa 100000 phần tử, do đó, quét tuyến tính vừa vặn thoải mái trong giới hạn, cả về thời gian chạy và bộ nhớ. Một phiên bản không gian cố định cũng có thể thực hiện được bằng cách tính toán các xu hướng một cách nhanh chóng, nhưng ở đây không cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    trend = []
    for i in range(n - 1):
        trend.append(1 if a[i] < a[i + 1] else -1)

    changes = 0
    for i in range(1, len(trend)):
        if trend[i] != trend[i - 1]:
            changes += 1

    return "YES\n" if changes == 1 else "NO\n"

# provided samples
assert run("5\n4 3 1 2 5\n") == "YES\n"
assert run("3\n1 3 2\n") == "YES\n"

# custom cases
assert run("3\n1 2 3\n") == "NO\n", "fully increasing"
assert run("3\n3 2 1\n") == "NO\n", "fully decreasing"
assert run("4\n2 1 3 4\n") == "YES\n", "single valley"
assert run("6\n1 3 2 4 5 6\n") == "NO\n", "multiple changes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | KHÔNG | tăng đơn điệu nên bị từ chối | 
| 3 2 1 | KHÔNG | giảm đơn điệu nên bị từ chối | 
| 2 1 3 4 | CÓ | cấu trúc thung lũng đơn là hợp lệ | 
| 1 3 2 4 5 6 | KHÔNG | thay đổi nhiều hướng phá vỡ hình kim tự tháp | 

## Vỏ cạnh 

Một chuỗi tăng nghiêm ngặt như`1 2 3 4`tạo ra một mảng xu hướng gồm tất cả +1 với 0 thay đổi, do đó nó bị từ chối ngay lập tức. Thuật toán không đếm lần lật nào và đưa ra kết quả KHÔNG chính xác. 

Một chuỗi giảm nghiêm ngặt như`4 3 2 1`hoạt động đối xứng, cũng tạo ra các lần lật bằng 0 và trả về NO. 

Một kim tự tháp hợp lệ tối thiểu như`2 1 3`tạo ra một thay đổi duy nhất từ ​​-1 đến +1. Thuật toán phát hiện chính xác một lần lật và trả về CÓ, phù hợp với yêu cầu cả hai phân đoạn đều tồn tại và không trống. 

Một chuỗi có nhiều dao động như`1 3 2 4`tạo ra hai lần lật trong mảng xu hướng, gây ra sự loại bỏ vì nó chứa nhiều hơn một cực trị, vi phạm ràng buộc hình chóp.
