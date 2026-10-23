---
title: "CF 105922J - Trò chơi chẵn lẻ"
description: "Chúng ta được cho hai số nguyên và chúng ta được biết một đặc tính cấu trúc chặt chẽ về chúng: một trong số chúng được đảm bảo là số lẻ và số còn lại được đảm bảo là số chẵn. Mỗi con số đại diện cho “sự lựa chọn của người chơi” trong một trò chơi so sánh rất đơn giản."
date: "2026-06-22T15:31:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105922
codeforces_index: "J"
codeforces_contest_name: "The 18th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 105922
solve_time_s: 53
verified: true
draft: false
---

[CF 105922J - Trò chơi Chẵn Lẻ](https://codeforces.com/problemset/problem/105922/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên và chúng ta được biết một đặc tính cấu trúc chặt chẽ về chúng: một trong số chúng được đảm bảo là số lẻ và số còn lại được đảm bảo là số chẵn. Mỗi con số đại diện cho “sự lựa chọn của người chơi” trong một trò chơi so sánh rất đơn giản. Một người chơi thích số lẻ, người kia thích số chẵn, nhưng điều duy nhất quan trọng cuối cùng là sự so sánh số giữa hai giá trị. 

Nhiệm vụ là xuất ra số nào trong hai số lớn hơn, nhưng đầu ra không phải là số đó. Thay vào đó, chúng tôi xuất ra`1`nếu số lớn hơn là số lẻ, và`2`nếu số lớn hơn là số chẵn. 

Các ràng buộc cực kỳ nhỏ về mặt thuật toán: các giá trị lên tới 10^9, dễ dàng phù hợp với các loại số nguyên tiêu chuẩn. Quan trọng hơn, chỉ có hai giá trị và không có cấu trúc ẩn như mảng, đồ thị hoặc nhiều truy vấn. Điều này ngay lập tức loại trừ mọi nhu cầu về tiền xử lý, cấu trúc dữ liệu hoặc thuật toán tiệm cận hiệu quả. Bất kỳ giải pháp nào so sánh trực tiếp hai số đều chạy trong thời gian không đổi. 

Không có trường hợp cạnh ẩn phức tạp nào về kích thước, nhưng có một cạm bẫy về mặt khái niệm: đầu ra được gắn với tính chẵn lẻ chứ không phải với vị trí. Một người đọc ngây thơ có thể giả định không chính xác “in 1 nếu a > b”, nhưng điều đó sẽ bỏ qua số lớn hơn là số lẻ hay số chẵn, điều này rất cần thiết cho việc lập bản đồ câu trả lời. 

Một ví dụ nhỏ cho thấy cấu trúc rõ ràng. Nếu đầu vào là`3 2`, số lẻ lớn hơn nên kết quả đầu ra là`1`. Nếu đầu vào là`1 2`, số chẵn lớn hơn nên kết quả là`2`. Sự nhầm lẫn chỉ nảy sinh nếu người ta quên rằng tính chẵn lẻ xác định nhãn nào sẽ được in chứ không phải số nào được in. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực vẫn có vẻ giống với cách giải thích tối ưu vì không có gì phải lặp lại. Người ta có thể tưởng tượng việc “thử nghiệm” cả hai khả năng, nhưng vì chỉ có hai con số nên mọi mô phỏng hoặc liệt kê đều suy biến thành một so sánh duy nhất, sau đó là bước phân loại. Ngay cả khi chúng ta đóng khung nó một cách giả tạo dưới dạng điều kiện kiểm tra cho cả hai số thì kết quả vẫn là O(1). 

Nhận xét quan trọng là bài toán không yêu cầu xây dựng bất cứ thứ gì, chỉ quyết định xem ứng cử viên nào trong hai ứng cử viên vừa lớn hơn vừa có tính chẵn lẻ đã biết. Vì chính xác một số là số lẻ và số kia là số chẵn nên chúng ta có thể xác định tính chẵn lẻ một cách an toàn bằng cách sử dụng phép toán mô đun và sau đó so sánh trực tiếp các giá trị. 

Ý tưởng mạnh mẽ “thử cả hai và quyết định” dẫn đến hiểu biết sâu sắc rằng câu trả lời được xác định bởi hai sự thật: số nào lớn hơn và số đó là số lẻ hay số chẵn. Một khi chúng ta nhận ra rằng cả hai dữ kiện đều có thể được tính toán trong thời gian không đổi thì bài toán sẽ trở thành bài kiểm tra có điều kiện trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên`a`Và`b`. Đây là những đầu vào duy nhất cần thiết để xác định kết quả. 
2. Kiểm tra xem số nào lớn hơn. Điều này xác định ứng cử viên nào sẽ “chiến thắng” theo nghĩa so sánh. 
3. Xác định tính chẵn lẻ của số lớn hơn bằng cách kiểm tra xem nó có chia hết cho 2 hay không. 
4. Nếu số lớn hơn là số lẻ, xuất ra`1`, vì người chơi số lẻ sẽ thắng cuộc so sánh. Ngược lại, xuất ra`2`, vì số lớn hơn phải là số chẵn. 

Bước lý luận quan trọng ở đây là tính chẵn lẻ không được sử dụng để quyết định thứ tự mà chỉ để ánh xạ người chiến thắng tới nhãn đầu ra được yêu cầu. Vì bài toán đảm bảo chính xác một số lẻ và một số chẵn nên không có sự mơ hồ trong ánh xạ này. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, chính xác một số là số lẻ và số kia là số chẵn, do đó danh tính của số lẻ được xác định rõ ràng. Thuật toán so sánh độ lớn một cách độc lập với tính chẵn lẻ, đảm bảo chọn đúng người chiến thắng. Khi giá trị lớn hơn được xác định, việc kiểm tra tính chẵn lẻ sẽ xác định duy nhất xem người chiến thắng đó tương ứng với người chơi chẵn hay lẻ. Vì cả hai bước đều mang tính xác định và bao gồm tất cả các đầu vào hợp lệ có thể có nên đầu ra phải khớp với quy tắc bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b = map(int, input().split())

mx = a if a > b else b

if mx % 2 == 1:
    print(1)
else:
    print(2)
```Việc triển khai được cố ý tối thiểu vì cấu trúc của bài toán không yêu cầu vòng lặp hoặc cấu trúc dữ liệu phụ trợ. Bước đầu tiên đọc trực tiếp hai số nguyên. Bước thứ hai chọn giá trị lớn hơn bằng cách sử dụng biểu thức điều kiện đơn giản, đủ vì chỉ có hai ứng cử viên. 

Kiểm tra tính chẵn lẻ`mx % 2 == 1`xác định xem mức tối đa có phải là số lẻ hay không. Vì bài toán đảm bảo chính xác một số lẻ và một số chẵn nên điều này sẽ tự động xác định số của người chơi nào đang được xuất ra. Câu lệnh in cuối cùng ánh xạ điều kiện này tới định dạng đầu ra được yêu cầu. 

Một sai lầm phổ biến là kiểm tra`a % 2`hoặc`b % 2`độc lập mà không cần xác định trước cái nào lớn hơn. Điều đó sẽ dẫn đến câu trả lời sai khi số lớn hơn là số chẵn nhưng không`a`, hoặc ngược lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1: nhập liệu`3 2`| Bước | một | b | mx | mx% 2 | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| đọc đầu vào | 3 | 2 | - | - | - | 
| so sánh | 3 | 2 | 3 | - | - | 
| kiểm tra tính chẵn lẻ | 3 | 2 | 3 | 1 | 1 | 

Ở đây tối đa là 3, là số lẻ nên đầu ra là 1. Điều này xác nhận rằng người tham gia có giá trị lẻ sẽ thắng khi họ cũng giữ số lớn hơn. 

### Ví dụ 2: nhập liệu`1 2`| Bước | một | b | mx | mx% 2 | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| đọc đầu vào | 1 | 2 | - | - | - | 
| so sánh | 1 | 2 | 2 | - | - | 
| kiểm tra tính chẵn lẻ | 1 | 2 | 2 | 0 | 2 | 

Ở đây tối đa là 2, là số chẵn nên đầu ra là 2. Điều này thể hiện trường hợp người tham gia chẵn sẽ thắng do có giá trị lớn hơn. 

Hai dấu vết này bao gồm cả các kết quả chẵn lẻ có thể có cho giá trị tối đa và xác nhận rằng thuật toán luôn ánh xạ người chiến thắng tới nhãn chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép so sánh và phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép tối đa 10^9, nhưng vì chỉ có hai số nguyên được xử lý nên thời gian chạy là không đổi và gần như phù hợp trong bất kỳ giới hạn thời gian nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b = map(int, input().split())
    mx = a if a > b else b
    return str(1 if mx % 2 == 1 else 2)

# provided samples
assert run("1 2") == "2"
assert run("3 2") == "1"

# custom cases
assert run("2 1") == "2", "even larger"
assert run("5 4") == "1", "odd larger"
assert run("8 7") == "2", "even larger"
assert run("9 2") == "1", "odd larger"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 2 | thậm chí còn lớn hơn và thắng | 
| 5 4 | 1 | số lẻ lớn hơn và thắng | 
| 8 7 | 2 | thậm chí thắng theo thứ tự ngược lại | 
| 9 2 | 1 | thắng lẻ với khoảng cách lớn | 

## Vỏ cạnh 

Điều kiện cạnh cấu trúc duy nhất là khi số lớn hơn là số chẵn, vì có thể dễ dàng vô tình trả về tính chẵn lẻ của biến sai. 

Đối với đầu vào`2 1`, thuật toán tính toán`mx = 2`, sau đó kiểm tra`2 % 2 == 0`, vì vậy nó xuất ra`2`. Một sai lầm ngây thơ sẽ là kiểm tra`a % 2`đầu tiên, hãy xem điều đó`2`là số chẵn và giả định sai rằng đầu ra phải là`2`mà không xác nhận điều đó`a`thực tế là tối đa trong mọi trường hợp. Logic đúng luôn gắn việc kiểm tra tính chẵn lẻ vào giá trị tối đa, điều này ngăn chặn sự phân loại sai này.
