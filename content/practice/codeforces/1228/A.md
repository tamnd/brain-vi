---
title: "CF 1228A - Chữ số riêng biệt"
description: "Chúng ta được cho một khoảng đóng gồm các số nguyên từ l đến r và chúng ta cần tìm bất kỳ số nào trong khoảng này mà biểu diễn thập phân của nó không lặp lại bất kỳ chữ số nào. Nói cách khác, khi viết số dưới dạng chuỗi chữ số, mỗi ký tự phải là duy nhất."
date: "2026-06-13T18:56:24+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1228
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 589 (Div. 2)"
rating: 800
weight: 1228
solve_time_s: 243
verified: true
draft: false
---

[CF 1228A - Các chữ số riêng biệt](https://codeforces.com/problemset/problem/1228/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 4m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một khoảng đóng các số nguyên từ`l`ĐẾN`r`và chúng ta cần tìm bất kỳ số nào trong khoảng này mà biểu diễn thập phân của nó không lặp lại bất kỳ chữ số nào. Nói cách khác, khi viết số dưới dạng chuỗi chữ số, mỗi ký tự phải là duy nhất. 

Nhiệm vụ không phải là tối ưu hóa cho bất kỳ thứ gì như giá trị tối thiểu hoặc tối đa. Bất kỳ số hợp lệ nào trong phạm vi đều có thể chấp nhận được, do đó cấu trúc của giải pháp có thể đơn giản miễn là nó tìm thấy một số như vậy một cách đáng tin cậy nếu nó tồn tại. 

Ràng buộc`r ≤ 100000`đủ nhỏ để có thể kiểm tra từng số trong khoảng đó. Kích thước khoảng tối đa là`10^5`và đối với mỗi số, chúng tôi chỉ kiểm tra tối đa 6 chữ số. Điều đó dẫn đến khoảng`6 * 10^5`hoạt động trong trường hợp xấu nhất, thoải mái trong giới hạn thời gian đối với Python. 

Một sai lầm ngây thơ thường xuất hiện khi người ta cố gắng xây dựng một con số một cách tham lam hoặc giả định theo một khuôn mẫu như luôn chọn`l`nếu nó hoạt động hoặc cố gắng "sửa" các chữ số trùng lặp theo từng chữ số. Những cách tiếp cận này có thể thất bại vì tính duy nhất của chữ số là thuộc tính chung. Ví dụ,`121`thất bại không phải vì vấn đề cục bộ mà vì chữ số`1`xuất hiện ở hai vị trí riêng biệt. 

Một trường hợp khó phát hiện khác là khi không có số nào trong khoảng hợp lệ. Ví dụ,`l = 110`Và`r = 120`chỉ chứa các số như`110, 111, 112 ...`, tất cả đều lặp lại chữ số. Trong những trường hợp như vậy, đầu ra chính xác là`-1`. 

Cuối cùng, các số có một chữ số luôn thỏa mãn điều kiện vì chỉ có một chữ số bắt đầu. Mọi triển khai đều phải xử lý các phạm vi bắt đầu từ`1`một cách chính xác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là lặp qua mọi số nguyên`x`từ`l`ĐẾN`r`và kiểm tra xem các chữ số của nó có khác biệt hay không. Việc kiểm tra một số rất đơn giản: chuyển đổi nó thành một chuỗi và xác minh rằng không có ký tự nào xuất hiện nhiều lần, thường sử dụng một bộ. 

Cách tiếp cận bạo lực này hoạt động vì kích thước đầu vào nhỏ. Trong trường hợp xấu nhất, chúng tôi kiểm tra`100000`số và mỗi séc quét tối đa 6 chữ số. Điều đó mang lại khối lượng công việc có giới hạn, hệ số không đổi nhỏ. 

Quan sát quan trọng là không có cấu trúc ẩn nào để khai thác. Ràng buộc được thiết kế có chủ ý sao cho vũ lực là giải pháp dự kiến. Mọi nỗ lực xây dựng các số theo tổ hợp hoặc bỏ qua các phạm vi đều không cần thiết. 

Do đó, chúng tôi giảm bớt vấn đề trong việc quét khoảng thời gian và xác nhận từng ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r - l + 1) * d) | O(1) | Đã chấp nhận | 
| Tối ưu | O((r - l + 1) * d) | O(1) | Đã chấp nhận | 

Đây`d ≤ 6`là số chữ số của mỗi số. 

## Hướng dẫn thuật toán 

1. Lặp lại mọi số nguyên`x`từ`l`ĐẾN`r`. Chúng tôi làm điều này vì bất kỳ câu trả lời hợp lệ nào cũng phải nằm trong phạm vi này và không có cấu trúc nào cho phép bỏ qua ứng viên một cách an toàn. 
2. Đối với mỗi`x`, chuyển đổi nó thành một biểu diễn chuỗi. Điều này cho phép chúng ta coi các chữ số là ký tự và đơn giản hóa việc kiểm tra tính duy nhất. 
3. Kiểm tra xem tất cả các chữ số trong chuỗi có khác biệt hay không. Chúng tôi duy trì một bộ và chèn từng chữ số khi quét. Nếu chúng tôi nhìn thấy một chữ số đã có trong tập hợp, chúng tôi sẽ từ chối số đó ngay lập tức. 
4. Nếu một số vượt qua quá trình kiểm tra tính duy nhất, hãy xuất số đó ngay lập tức và kết thúc. Điều này hợp lệ vì mọi giải pháp đều được chấp nhận. 
5. Nếu vòng lặp kết thúc mà không tìm thấy số nào hợp lệ, xuất ra`-1`. 

### Tại sao nó hoạt động 

Mỗi số ứng cử viên được xác minh độc lập cho thuộc tính được yêu cầu và mỗi số trong khoảng được kiểm tra chính xác một lần. Vì việc kiểm tra là chính xác và không có phép biến đổi nào được áp dụng nên không có ứng cử viên hợp lệ nào có thể bị bỏ qua. Thuật toán không thể chấp nhận sai một số không hợp lệ vì sự lặp lại được phát hiện rõ ràng bằng cách sử dụng bài kiểm tra tư cách thành viên đã đặt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    l, r = map(int, input().split())
    
    for x in range(l, r + 1):
        s = str(x)
        seen = set()
        ok = True
        for ch in s:
            if ch in seen:
                ok = False
                break
            seen.add(ch)
        if ok:
            print(x)
            return
    
    print(-1)

if __name__ == "__main__":
    solve()
```Vòng lặp lõi trực tiếp thực hiện quét brute-force. Việc chuyển đổi thành chuỗi chỉ được sử dụng để trích xuất chữ số và bộ này đảm bảo phát hiện các bản sao theo thời gian liên tục. Việc thoát sớm rất quan trọng vì khi tìm thấy số hợp lệ thì việc tìm kiếm thêm là không cần thiết. 

Điều kiện biên duy nhất cần cẩn thận là phạm vi bao gồm`r + 1`trong Python`range`, đảm bảo điểm cuối được kiểm tra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
121 130
```Chúng tôi kiểm tra các số một cách tuần tự: 

| x | chữ số | quá trình nhìn thấy | hợp lệ | 
| --- | --- | --- | --- | 
| 121 | 1,2,1 | lặp lại 1 | không | 
| 122 | 1,2,2 | lặp lại 2 | không | 
| 123 | 1,2,3 | tất cả đều độc đáo | vâng | 

Một lần`123`được tìm thấy, chúng tôi dừng lại. 

Điều này xác nhận hành vi kết thúc sớm và cho thấy thuật toán dừng ở ứng viên hợp lệ đầu tiên như thế nào. 

### Ví dụ 2 

đầu vào:```
110 115
```| x | chữ số | quá trình nhìn thấy | hợp lệ | 
| --- | --- | --- | --- | 
| 110 | 1,1,0 | lặp lại 1 | không | 
| 111 | 1,1,1 | lặp lại 1 | không | 
| 112 | 1,1,2 | lặp lại 1 | không | 
| 113 | 1,1,3 | lặp lại 1 | không | 
| 114 | 1,1,4 | lặp lại 1 | không | 
| 115 | 1,1,5 | lặp lại 1 | không | 

Không có số hợp lệ tồn tại, vì vậy đầu ra là`-1`. 

Điều này thể hiện trường hợp thất bại toàn diện trong đó mọi ứng viên đều bị từ chối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((r - l + 1) * d) | Chúng tôi quét từng số trong khoảng và kiểm tra từng chữ số một lần | 
| Không gian | O(1) | Bộ chứa tối đa 10 chữ số | 

Số lần lặp tối đa là`10^5`và mỗi lần lặp xử lý tối đa 6 chữ số, do đó, giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    l, r = map(int, input().split())
    for x in range(l, r + 1):
        s = str(x)
        seen = set()
        ok = True
        for ch in s:
            if ch in seen:
                ok = False
                break
            seen.add(ch)
        if ok:
            print(x)
            return
    print(-1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    
    solve()
    
    out = sys.stdout.getvalue().strip()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# provided sample
assert run("121 130") == "123"

# custom cases
assert run("1 9") == "1", "single digit always valid"
assert run("11 11") == "-1", "all digits repeated"
assert run("98 102") in ["98", "102"], "cross boundary behavior"
assert run("123 123") == "123", "already valid single value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 9 | 1 | phạm vi nhỏ nhất và thành công ngay lập tức | 
| 11 11 | -1 | điều kiện lỗi giá trị đơn | 
| 98 102 | 98 hoặc 102 | vượt qua ranh giới và linh hoạt | 
| 123 123 | 123 | trường hợp hợp lệ tầm thường | 

## Vỏ cạnh 

Đối với một phạm vi giá trị đơn như`11 11`, thuật toán chỉ kiểm tra số đó, phát hiện chữ số lặp lại`1`và đầu ra chính xác`-1`. 

Đối với phạm vi trong đó số đầu tiên hợp lệ, chẳng hạn như`1 9`, thuật toán chấp nhận ngay`1`và dừng lại, cho thấy rằng việc chấm dứt sớm sẽ ngăn chặn việc quét không cần thiết. 

Đối với các chuyển tiếp ranh giới như`98 102`, thuật toán xử lý chính xác các độ dài chữ số khác nhau mà không cần bất kỳ cách viết hoa đặc biệt nào, vì tất cả các số đều được xử lý thống nhất dưới dạng chuỗi. 

Đối với các trường hợp suy biến trong đó tất cả các ứng cử viên đều không hợp lệ, như`110 115`, mỗi lần lặp đều thất bại trong việc kiểm tra tính duy nhất và thuật toán sẽ kết thúc vòng lặp một cách chính xác và trả về`-1`.
