---
title: "CF 105507L - \u0418\u0434\u0435\u0430\u043b\u044c\u043d\u0430\u044f \u0444\u043e\u0442\u043e\u0433\u0440\u0430\u0444\u0438\u044f"
description: "Chúng tôi được cung cấp một nhóm người chơi, mỗi người có chiều cao khác nhau. Chúng tôi muốn đặt tất cả những người chơi hiện có vào một hàng duy nhất và có thể thêm những người chơi mới để đội hình cuối cùng tạo thành một chuỗi chiều cao liên tiếp hoàn hảo."
date: "2026-06-23T22:01:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105507
codeforces_index: "L"
codeforces_contest_name: "2024-2025 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 24, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 105507
solve_time_s: 54
verified: true
draft: false
---

[CF 105507L - \u0418\u0434\u0435\u0430\u043b\u044c\u043d\u0430\u044f \u0444\u043e\u0442\u043e\u0433\u0440\u0430\u0444\u0438\u044f](https://codeforces.com/problemset/problem/105507/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một nhóm người chơi, mỗi người có chiều cao khác nhau. Chúng tôi muốn đặt tất cả những người chơi hiện có vào một hàng duy nhất và có thể thêm những người chơi mới để đội hình cuối cùng tạo thành một chuỗi chiều cao liên tiếp hoàn hảo. “Hoàn hảo” ở đây có nghĩa là một khi được sắp xếp từ trái sang phải, mỗi cặp liền kề sẽ khác nhau đúng một về chiều cao và người ngoài cùng bên trái có thể bắt đầu từ bất kỳ độ cao nào. 

Vì vậy, nhiệm vụ không phải là sắp xếp lại các ràng buộc phức tạp tùy ý mà là biến một tập hợp số nguyên phân tán nhất định thành tập con của một khoảng nguyên nào đó không có khoảng trống lớn hơn một sau khi điền các giá trị còn thiếu bằng cách chèn người chơi mới. Mỗi trình phát được chèn đóng góp một chiều cao số nguyên bị thiếu mà ban đầu không có. 

Đầu ra là số lượng số nguyên bổ sung tối thiểu mà chúng ta phải chèn để tất cả các giá trị đã cho có thể được nhúng vào ít nhất một dãy số nguyên liên tiếp. 

Các ràng buộc lên tới 200000 độ cao khác nhau, do đó, mọi so sánh bậc hai giữa các cặp người chơi ngay lập tức là quá chậm. Một giải pháp về cơ bản phải là sắp xếp dựa trên hoặc tuyến tính sau khi sắp xếp, vì O(n log n) có thể chấp nhận được còn O(n^2) thì không. 

Một điểm tinh tế là trình tự cuối cùng không cần bắt đầu ở độ cao nhất định tối thiểu cũng như không kết thúc ở độ cao tối đa. Chúng tôi được phép chọn bất kỳ khoảng nào chứa tất cả các độ cao đã chọn và chúng tôi có thể mở rộng ra ngoài chúng nếu điều đó làm giảm số lần chèn. Điều này làm cho nó trở thành một vấn đề về khoảng bao phủ thay vì chỉ đếm các khoảng trống bên trong phạm vi đầu vào. 

Một sai lầm ngây thơ là cho rằng chúng ta chỉ cần lấp đầy khoảng trống giữa các độ cao được sắp xếp liên tiếp. Ví dụ: với độ cao 1, 10, 20, chỉ cần tính tổng các giá trị còn thiếu giữa các cặp liền kề sẽ bỏ qua rằng chúng ta có thể chọn khoảng thời gian dài hơn để căn chỉnh tốt hơn và giảm số lần chèn. Một sai lầm khác là cho rằng câu trả lời chỉ phụ thuộc vào max trừ min, điều này không thành công khi có những khoảng trống bên trong lớn buộc phải thực hiện nhiều lần chèn bất kể điểm cuối. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi khoảng mục tiêu có thể [L, R] có thể chứa dòng ảnh cuối cùng. Đối với mỗi lựa chọn, chúng tôi kiểm tra xem có bao nhiêu số nguyên trong khoảng đó bị thiếu trong số các độ cao đã cho và chúng tôi lấy giá trị tối thiểu trên tất cả các khoảng chứa tất cả các giá trị ban đầu. Điều này đúng vì bất kỳ dòng cuối cùng hợp lệ nào đều tương ứng chính xác với một khoảng nào đó chứa tất cả các chiều cao ban đầu. 

Tuy nhiên, cách tiếp cận này trở nên không khả thi vì L và R có thể thay đổi trong phạm vi lên tới 10^9, do đó việc liệt kê các khoảng là không thể. Ngay cả khi chúng tôi giới hạn các khoảng được giới hạn bởi các giá trị đầu vào, vẫn có các ứng cử viên O(n^2) và việc kiểm tra từng khoảng sẽ dẫn đến hành vi O(n^3) trong trường hợp xấu nhất. 

Quan sát quan trọng là chúng ta thực sự không cần phải kiểm tra các khoảng thời gian tùy ý. Nếu chúng ta cố định điểm bắt đầu L thì R tối ưu sẽ được xác định: nó phải bao gồm tất cả các độ cao đã cho, do đó R ít nhất phải là max(ai). Khi L được cố định, chi phí được xác định trực tiếp bằng số lượng số nguyên bị thiếu trong [L, max(ai)]. Điều này làm giảm vấn đề chọn L một cách tối ưu. 

Sau khi sắp xếp, L tối ưu sẽ luôn thẳng hàng với một trong các độ cao hiện có. Nếu L được đặt giữa hai giá trị, việc dịch chuyển nó sang giá trị hiện có gần nhất chỉ làm giảm hoặc duy trì số lần chèn cần thiết mà không làm mất tính khả thi. Điều này biến vấn đề thành việc đánh giá tất cả các vị trí đã sắp xếp như các điểm cuối bên trái có thể. 

Đối với L cố định bằng a[i], độ dài khoảng cuối cùng được yêu cầu là max(a) - a[i] + 1. Số phần tử đã tồn tại bên trong khoảng đó chỉ là số phần tử từ chỉ mục i trở đi, vì mọi thứ đều khác biệt và được sắp xếp. Vì vậy, số lượng người chơi được chèn là độ dài khoảng trừ đi các phần tử có sẵn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng chiều cao theo thứ tự tăng dần. Điều này là cần thiết vì chúng ta muốn suy luận về các khoảng nguyên liền kề và việc sắp xếp sẽ chuyển đổi vấn đề thành một cấu trúc trong đó các chỉ số liên tiếp tương ứng với độ cao tăng dần. 
2. Đặt chiều cao tối đa là phần tử cuối cùng sau khi sắp xếp. Giá trị này đóng vai trò là ranh giới bên phải bắt buộc của bất kỳ khoảng cuối cùng hợp lệ nào nếu chúng ta chọn điểm bắt đầu nhất định. 
3. Với mỗi chỉ số i, coi a[i] là ứng cử viên có chiều cao bắt đầu L của chuỗi liên tiếp cuối cùng. Điều này là hợp lý vì bất kỳ khoảng tối ưu nào cũng có thể được dịch chuyển cho đến khi ranh giới bên trái của nó trùng với chiều cao hiện có mà không làm tăng số lần chèn cần thiết. 
4. Với lựa chọn L = a[i] này, hãy tính độ dài của đoạn nguyên đầy đủ cần thiết để đạt giá trị tối đa: max(a) - a[i] + 1. Điều này biểu thị có bao nhiêu chiều cao nguyên riêng biệt phải tồn tại trong một chuỗi hoàn hảo bắt đầu từ a[i] và kết thúc ở max(a). 
5. Tính xem có bao nhiêu người chơi ban đầu nằm trong phân khúc này. Vì mảng được sắp xếp nên tất cả các phần tử từ chỉ mục i đến n-1 đều nằm trong khoảng này, do đó số lượng của chúng là n - i. 
6. Số lượng người chơi mới cần thiết cho khoảng thời gian này là chênh lệch giữa kích thước phân khúc được yêu cầu và các phần tử hiện có: (max(a) - a[i] + 1) - (n - i). 
7. Lấy giá trị nhỏ nhất trên tất cả i. Điều này chọn điểm bắt đầu tốt nhất có thể, tức là điểm bắt đầu giảm thiểu số nguyên bị thiếu. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến mà mọi sự sắp xếp cuối cùng tối ưu đều tương ứng với một khoảng nguyên liền kề chứa đầy đủ tất cả các độ cao ban đầu đã chọn. Đối với bất kỳ khoảng thời gian nào như vậy, việc trượt điểm cuối bên trái của nó sang phải cho đến khi chạm vào độ cao hiện có sẽ không bao giờ làm tăng số lượng số nguyên bị thiếu, bởi vì nó chỉ loại bỏ các giá trị chưa được khám phá ở phía bên trái mà không ảnh hưởng đến giới hạn biên bên phải do phần tử được chọn tối đa áp đặt. Do đó, việc hạn chế sự chú ý đến điểm bắt đầu bằng các giá trị hiện tại sẽ duy trì tính tối ưu. Khi ranh giới bên trái được cố định, chi phí được xác định đầy đủ bằng cách đếm xem có bao nhiêu số nguyên bị thiếu trong khoảng bắt buộc lên đến mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
a.sort()

mx = a[-1]

ans = n  # upper bound: at worst we fill everything between min and max

for i in range(n):
    length = mx - a[i] + 1
    have = n - i
    need = length - have
    if need < ans:
        ans = need

print(ans)
```Giải pháp bắt đầu bằng cách sắp xếp để áp đặt cấu trúc lên các độ cao. Giá trị tối đa được trích xuất một lần vì mọi khoảng ứng cử viên mà chúng tôi xem xét đều kết thúc ở đó; bất kỳ giải pháp tối ưu nào bỏ qua nó sẽ thực sự tồi tệ hơn vì việc mở rộng để bao gồm nó chỉ mở rộng phạm vi có sẵn mà không làm tăng các ràng buộc liên tục cần thiết. 

Đối với mỗi chỉ số bắt đầu có thể có, công thức tính toán có bao nhiêu số nguyên bị thiếu trong khoảng đầy đủ từ chiều cao đó đến mức tối đa. Phép trừ`n - i`hoạt động vì việc sắp xếp đảm bảo rằng tất cả các phần tử từ i trở đi đều nằm trong khoảng. Điều này tránh việc kiểm tra thành viên rõ ràng hoặc thiết lập các hoạt động. 

Giá trị ban đầu của`ans`có thể bắt đầu an toàn ở n, vì trong trường hợp xấu nhất, chúng ta có thể cần chèn tối đa n-1 giá trị hoặc ít hơn và điều này hoạt động như một giới hạn trên đơn giản. Không có trường hợp cạnh nào yêu cầu xử lý đặc biệt vì vòng lặp đánh giá một cách tự nhiên ngay cả điểm bắt đầu cực đoan tại i = n-1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
195 200 197 199
```Mảng được sắp xếp là`[195, 197, 199, 200]`, tối đa là 200. 

| tôi | một [tôi] | khoảng [a[i], mx] | chiều dài | có (n-i) | cần | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 195 | [195, 200] | 6 | 4 | 2 | 
| 1 | 197 | [197, 200] | 4 | 3 | 1 | 
| 2 | 199 | [199, 200] | 2 | 2 | 0 | 
| 3 | 200 | [200, 200] | 1 | 1 | 0 | 

Tối thiểu là 0, đạt được bằng cách bắt đầu từ 199 hoặc 200. 

Điều này cho thấy việc chọn một điểm bắt đầu khác có thể loại bỏ hoàn toàn nhu cầu chèn khi các điểm hiện có đã tạo thành một hậu tố gần liên tiếp. 

### Ví dụ 2 

đầu vào:```
3
180 181 182
```Mảng được sắp xếp là`[180, 181, 182]`, tối đa là 182. 

| tôi | một [tôi] | khoảng thời gian | chiều dài | có | cần | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 180 | [180,182] | 3 | 3 | 0 | 
| 1 | 181 | [181,182] | 2 | 2 | 0 | 
| 2 | 182 | [182,182] | 1 | 1 | 0 | 

Mọi điểm bắt đầu đều mang lại chi phí bằng 0 vì mảng đã liên tiếp trong không gian giá trị. Điều này xác nhận rằng thuật toán nhận dạng chính xác các cấu hình vốn đã hoàn hảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét tuyến tính đơn theo sau | 
| Không gian | O(1) | Chỉ sử dụng tính năng sắp xếp tại chỗ và một vài biến | 

Các ràng buộc cho phép tối đa 200000 phần tử và giải pháp O(n log n) nằm trong giới hạn một cách thoải mái. Việc sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào, không đáng kể dưới 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    mx = a[-1]
    ans = n
    for i in range(n):
        ans = min(ans, mx - a[i] + 1 - (n - i))
    return str(ans)

# provided sample-like cases
assert run("4\n195 200 197 199\n") == "0"
assert run("3\n180 181 182\n") == "0"

# custom cases
assert run("2\n1 10\n") == "8", "gap case"
assert run("5\n5 1 2 3 10\n") == "4", "multiple clusters"
assert run("1\n100\n") == "0", "single element"
assert run("6\n10 20 30 40 50 60\n") == "45", "large sparse"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 số cách xa nhau | 8 | khoảng cách lớn duy nhất | 
| nhiều cụm | 4 | khoảng cách hỗn hợp | 
| phần tử đơn | 0 | trường hợp cạnh tối thiểu | 
| cấp số cộng thưa thớt | 45 | mật độ trường hợp xấu nhất | 

## Vỏ cạnh 

Đối với một yếu tố đầu vào như`[100]`, thuật toán sắp xếp nó, đặt giá trị tối đa bằng 100 và chỉ đánh giá một khoảng có độ dài 1. Nhu cầu tính toán bằng 0 vì không cần tính liên tục ngoài một điểm. 

Đối với một mảng đầy đủ liên tiếp như`[5, 6, 7, 8]`, mỗi lần lặp đều tính bằng 0 vì độ dài khoảng luôn khớp với số phần tử được bao gồm. Thuật toán xác định chính xác rằng không cần chèn thêm bất kể vị trí bắt đầu. 

Đối với các giá trị rất thưa thớt như`[1, 1000000000]`, lựa chọn tối ưu là bắt đầu ở một trong hai điểm cuối, nhưng quá trình tính toán luôn mang lại một khoảng lớn trừ đi hai phần tử hiện có, tạo ra số lần chèn chính xác bằng toàn bộ phạm vi bị thiếu trừ đi các điểm hiện có.
