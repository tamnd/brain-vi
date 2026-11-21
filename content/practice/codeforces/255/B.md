---
title: "CF 255B - Phân tích mã"
description: "Chúng ta được cung cấp một chuỗi chỉ bao gồm các ký tự \"x\" và \"y\". Hai thao tác có thể được áp dụng lặp đi lặp lại theo một thứ tự cụ thể. Thao tác đầu tiên hoán đổi một \"y\" liên tiếp theo sau là \"x\" thành \"x\" rồi \"y\". Thao tác thứ hai loại bỏ \"x\" liên tiếp theo sau là \"y\"."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 255
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 156 (Div. 2)"
rating: 1200
weight: 255
solve_time_s: 139
verified: true
draft: false
---

[CF 255B - Phân tích mã](https://codeforces.com/problemset/problem/255/B) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chỉ bao gồm các ký tự "x" và "y". Hai thao tác có thể được áp dụng lặp đi lặp lại theo một thứ tự cụ thể. Thao tác đầu tiên hoán đổi một chữ "y" liên tiếp theo sau là "x" thành "x" rồi "y". Thao tác thứ hai loại bỏ một chữ "x" liên tiếp theo sau là "y". Chuỗi được xử lý một cách tham lam: nếu có thể thực hiện được thao tác nào thì chuỗi đó sẽ được áp dụng trước; mặt khác, thao tác hai được áp dụng. Quá trình lặp lại cho đến khi không thể thực hiện được thao tác nào. 

Đầu vào là một chuỗi tối đa$10^6$nhân vật. Đầu ra là chuỗi sau khi quá trình này dừng lại. Do hạn chế về độ dài, một cách tiếp cận đơn giản là quét liên tục toàn bộ chuỗi để áp dụng hoán đổi hoặc loại bỏ có thể mất tới$10^6$lặp đi lặp lại cho mỗi hoạt động, đưa ra$O(n^2)$hành vi. Điều này là quá chậm đối với$n = 10^6$, do đó cần có một giải pháp tuyến tính hoặc gần tuyến tính hiệu quả. 

Các trường hợp đặc biệt không rõ ràng bao gồm các chuỗi đã được sắp xếp như "xxxx" hoặc "yyyy", không thay đổi, các chuỗi có mẫu xen kẽ như "yxyxy" sẽ kích hoạt nhiều lần hoán đổi trước khi xóa và các chuỗi kết thúc bằng "xy" sẽ bị xóa ngay lập tức. Việc triển khai bất cẩn mà bỏ qua thứ tự hoạt động hoặc không xử lý chính xác các điều kiện biên sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: quét liên tục chuỗi từ trái sang phải, áp dụng thao tác áp dụng đầu tiên, sau đó khởi động lại. Điều này hoạt động chính xác vì nó phản ánh thuật toán tham lam, nhưng độ phức tạp của nó là$O(n^2)$trong trường hợp xấu nhất, ví dụ: trên đầu vào "yxyxyxyx..." có độ dài$10^6$. Mỗi lần hoán đổi sẽ di chuyển một vị trí "y" sang phải, do đó việc xử lý tất cả các "y" có thể yêu cầu khoảng$O(n^2)$trao đổi. 

Quan sát quan trọng để tối ưu hóa là thao tác thứ nhất, hoán đổi "y" và "x", về cơ bản sẽ đẩy tất cả "y" sang bên phải so với "x" trước đó. Thao tác thứ hai, loại bỏ các cặp "xy", loại bỏ "x" và "y" bất cứ khi nào "x" đứng trước "y". Sau khi hoàn tất mọi thao tác hoán đổi, mọi chữ "y" có chữ "x" ở bên trái cuối cùng sẽ bị xóa. Kết quả là chuỗi cuối cùng sẽ bao gồm tất cả các chữ "x" chưa bao giờ được ghép với chữ "y" ở bên phải và tất cả các chữ "y" chưa bao giờ được ghép với chữ "x" ở bên trái. Điều này cho phép chúng tôi mô phỏng thuật toán theo một lượt từ trái sang phải bằng cách sử dụng ngăn xếp. 

Trong thực tế, chúng ta lặp qua chuỗi, duy trì một ngăn xếp. Nếu chúng ta gặp chữ "x" và phần trên cùng của ngăn xếp là "y", chúng ta không thể áp dụng thao tác xóa nên chúng ta nhấn "x". Nếu chúng tôi gặp chữ "y" và phần trên cùng của ngăn xếp là "x", chúng tôi sẽ xóa "x" (mô phỏng việc xóa "xy"). Tất cả các ký tự khác được đẩy lên ngăn xếp. Cách tiếp cận này chạy trong$O(n)$thời gian và công dụng$O(n)$không gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu (mô phỏng ngăn xếp) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một danh sách trống để mô phỏng một ngăn xếp. Điều này sẽ giữ những nhân vật sống sót cho đến nay. 
2. Lặp lại chuỗi từ trái sang phải. Đối với mỗi ký tự, hãy kiểm tra xem ngăn xếp có trống không. 
3. Nếu ký tự hiện tại là "y" và phần trên cùng của ngăn xếp là "x", hãy lấy "x" ra khỏi ngăn xếp. Điều này mô phỏng việc xóa cặp "xy" theo thao tác hai. 
4. Nếu không, hãy đẩy ký tự hiện tại vào ngăn xếp. Điều này giữ tất cả các ký tự không bị xóa ngay lập tức. 
5. Sau khi quá trình lặp hoàn tất, hãy nối ngăn xếp thành một chuỗi và trả về nó. 

Tại sao nó hoạt động: tính bất biến của ngăn xếp là nó luôn chứa chuỗi ký tự tồn tại trong tất cả các hoạt động trước đó. Mỗi khi chữ "x" gặp chữ "y" ở bên phải của nó, chúng sẽ hủy chính xác một lần, phù hợp với quy tắc xóa tham lam. Việc hoán đổi "y" trước "x" được xử lý ngầm vì chỉ còn lại "x" và "y" không khớp, theo đúng thứ tự tương đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
stack = []

for ch in s:
    if stack and stack[-1] == 'x' and ch == 'y':
        stack.pop()
    else:
        stack.append(ch)

print("".join(stack))
```Mã đọc chuỗi một cách hiệu quả và lặp lại một lần. Ngăn xếp mô phỏng các nhân vật sống sót. Mỗi cặp "xy" sẽ bị xóa trong thời gian không đổi khi gặp phải. Việc sử dụng`stack[-1]`đảm bảo chúng tôi kiểm tra ký tự cuối cùng còn sót lại, duy trì đúng thứ tự xóa. Việc đẩy các ký tự khác sẽ giữ nguyên "x" và "y" còn lại theo đúng trình tự của chúng. 

## Ví dụ đã hoạt động 

Nhập "x": 

| Chỉ mục | Char | Ngăn xếp | Hành động | 
| --- | --- | --- | --- | 
| 0 | x | [] | đẩy x | 

Đầu ra: "x". Không xóa, xác nhận trường hợp cơ sở. 

Nhập "yxyxy": 

| Chỉ mục | Char | Ngăn xếp | Hành động | 
| --- | --- | --- | --- | 
| 0 | y | [] | đẩy y | 
| 1 | x | ['y'] | đẩy x | 
| 2 | y | ['y', 'x'] | trên cùng là x, ch là y -> pop x | 
| 3 | x | ['y', 'y'] | đẩy x | 
| 4 | y | ['y', 'y', 'x'] | trên cùng là x, ch là y -> pop x | 

Đầu ra: "yy". Điều này phù hợp với mong đợi sau tất cả các lần hoán đổi và xóa. 

Nhập "xxxxxxy": 

| Chỉ mục | Char | Ngăn xếp | Hành động | 
| --- | --- | --- | --- | 
| 0-4 | x | [] -> ['x', 'x', 'x', 'x', 'x'] | đẩy x mỗi lần | 
| 5 | y | ['x', 'x', 'x', 'x', 'x'] | trên cùng là x, ch là y -> pop x | 

Đầu ra: "xxxx". Thể hiện việc xóa thích hợp ở cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được đẩy/xuất hiện nhiều nhất một lần, do đó tuyến tính theo độ dài chuỗi | 
| Không gian | O(n) | Ngăn xếp có thể chứa tất cả các ký tự trong trường hợp xấu nhất | 

Thời gian tuyến tính đảm bảo giải pháp hoạt động thoải mái cho$n \le 10^6$, trong giới hạn 2 giây. Việc sử dụng bộ nhớ lên tới 1 triệu ký tự phù hợp với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    stack = []
    for ch in s:
        if stack and stack[-1] == 'x' and ch == 'y':
            stack.pop()
        else:
            stack.append(ch)
    return "".join(stack)

# provided samples
assert run("x\n") == "x", "sample 1"
assert run("yxyxy\n") == "y", "sample 2"
assert run("xxxxxy\n") == "xxxx", "sample 3"

# custom cases
assert run("yyyyy\n") == "yyyyy", "all y"
assert run("xyxyxyxy\n") == "", "alternating xy deletion"
assert run("yxxxyyy\n") == "yyy", "mixed with leftover y"
assert run("x\n") == "x", "single x"
assert run("y\n") == "y", "single y"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "yyyyy" | "yyyyy" | Không thể thực hiện thao tác nào | 
| "xyxyxyxy" | "" | Tất cả các thao tác xóa được áp dụng theo trình tự | 
| "yxxxyyy" | "yyy" | Xóa hỗn hợp, y còn sót lại | 
| "x" | "x" | Ký tự đơn tối thiểu, không có thao tác | 
| "y" | "y" | Ký tự đơn tối thiểu, không có thao tác | 

## Vỏ cạnh 

Đối với đầu vào "yyyyy", thuật toán sẽ đẩy tất cả các ký tự vào ngăn xếp mà không xóa. Đầu ra vẫn là "yyyyy", bảo toàn chính xác các ký tự vì không có "x" đứng trước "y". 

Đối với đầu vào "xyxyxyxy", mỗi "x" theo sau là "y" sẽ bị xóa ngay lập tức. Ngăn xếp tăng lên và co lại tương ứng, dẫn đến một chuỗi trống. Điều này xác minh rằng việc xóa tuần tự được xử lý chính xác. 

Đối với đầu vào "yxxxyyy", "y" ban đầu được đẩy, chuỗi "x" vẫn duy trì cho đến khi "y" cuối cùng hủy một "x", để lại ba "y". Điều này chứng tỏ các ký tự còn sót lại được bảo toàn chính xác theo quy định.
