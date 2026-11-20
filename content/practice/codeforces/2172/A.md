---
title: "CF 2172A - Cuộc thi nghệ thuật ASCII"
description: "Chúng tôi nhận được điểm số của ba trợ lý sáng tạo được hỗ trợ bởi AI-Gemini, ChatGPT và Claude-trong một cuộc thi nghệ thuật ASCII. Mỗi điểm là một số nguyên từ 80 đến 100. Ban tổ chức muốn quyết định xem điểm của ban giám khảo có đủ nhất quán để công bố kết quả cuối cùng hay không."
date: "2026-06-07T22:53:19+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2172
codeforces_index: "A"
codeforces_contest_name: "2025 ICPC Asia Taichung Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 800
weight: 2172
solve_time_s: 174
verified: true
draft: false
---

[CF 2172A - Cuộc thi nghệ thuật ASCII](https://codeforces.com/problemset/problem/2172/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được điểm số của ba trợ lý sáng tạo được hỗ trợ bởi AI-Gemini, ChatGPT và Claude-trong một cuộc thi nghệ thuật ASCII. Mỗi điểm là một số nguyên từ 80 đến 100. Ban tổ chức muốn quyết định xem điểm của ban giám khảo có đủ nhất quán để công bố kết quả cuối cùng hay không. Luật chơi rất đơn giản: nếu chênh lệch giữa điểm cao nhất và thấp nhất ít nhất là 10 thì ban giám khảo phải “kiểm tra lại”; nếu không, kết quả được công bố là điểm trung bình của ba điểm. Trung vị ở đây biểu thị điểm sẽ xuất hiện ở giữa nếu ba điểm được sắp xếp. 

Đầu vào là một dòng gồm ba số nguyên biểu thị điểm số và đầu ra là một dòng duy nhất, "kiểm tra lại" hoặc "X cuối cùng", trong đó X là trung vị. Các ràng buộc rất nhỏ: chỉ có ba số nguyên, vì vậy bất kỳ thuật toán nào chúng tôi chọn sẽ chạy trong thời gian không đổi. Tuy nhiên, các trường hợp đặc biệt không rõ ràng bao gồm cả ba điểm bằng nhau, hai điểm ở một điểm cực trị và một điểm ở điểm kia hoặc điểm khác nhau đúng 10. Ví dụ: nếu điểm là 80, 90, 90 thì chênh lệch giữa điểm tối đa và điểm tối thiểu chính xác là 10, vì vậy chương trình sẽ in "kiểm tra lại". Một cách tiếp cận bất cẩn có thể tính toán trung vị không chính xác bằng cách chọn sai phần tử ở giữa hoặc sử dụng bất đẳng thức loại trừ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực ở đây là sắp xếp ba điểm. Việc sắp xếp danh sách ba phần tử là chuyện nhỏ; sau đó chúng ta có thể trực tiếp chọn giá trị ở giữa làm giá trị trung bình và kiểm tra sự khác biệt giữa mức tối đa và mức tối thiểu. Điều này hiệu quả vì việc sắp xếp đảm bảo chúng ta có thể xác định giá trị tối thiểu, tối đa và trung vị một cách đáng tin cậy. Mặc dù việc sắp xếp thường là O(n log n), nhưng với ba phần tử, nó thực sự là thời gian không đổi, do đó hiệu suất không phải là vấn đề đáng lo ngại. 

Một cách tiếp cận tối ưu hơn một chút sẽ tránh được việc sắp xếp đầy đủ. Chúng ta có thể tính toán mức tối thiểu, tối đa và trung bình bằng logic điều kiện đơn giản. Điểm tối thiểu là điểm nhỏ nhất trong ba điểm, điểm tối đa là điểm lớn nhất và có thể tìm thấy điểm trung bình bằng cách cộng cả ba điểm và trừ đi điểm tối thiểu và tối đa. Điều này mang lại kết quả tương tự trong ít so sánh hơn và thanh lịch. Việc quan sát rằng trung vị của ba số bằng tổng trừ đi số nhỏ nhất và số lớn nhất cho phép chúng ta tránh được việc sắp xếp hoàn toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sắp xếp ba điểm | O(1) | O(1) | Đã chấp nhận | 
| Thủ thuật tối thiểu/tối đa/tổng ​​| O(1) | O(1) | Đã chấp nhận | 

Cả hai cách tiếp cận đều có thời gian không đổi một cách hiệu quả vì kích thước đầu vào được cố định ở mức ba. 

## Hướng dẫn thuật toán 

1. Đọc ba điểm thành biến`g`,`c`, Và`l`. Đây là ánh xạ trực tiếp của chúng tôi từ đầu vào đến biểu diễn bên trong. 
2. Tính điểm tối đa`mx`sử dụng`max(g, c, l)`. Điều này xác định trợ lý AI có điểm số cao nhất. 
3. Tính điểm tối thiểu`mn`sử dụng`min(g, c, l)`. Điều này xác định trợ lý AI có điểm thấp nhất. 
4. Nếu sự khác biệt`mx - mn`lớn hơn hoặc bằng 10, in "kiểm tra lại" và dừng. Điều này thực thi quy tắc rằng điểm số quá chênh lệch. 
5. Ngược lại, tính điểm trung bình như sau:`g + c + l - mx - mn`. Điều này có tác dụng vì tổng số trừ đi giá trị tối thiểu và tối đa sẽ để lại giá trị ở giữa. 
6. In "cuối cùng" theo sau là điểm trung bình. 

Tại sao nó hoạt động: Thuật toán xác định rõ ràng các giá trị cực trị để kiểm tra tiêu chí nhất quán và thủ thuật tổng-trừ-tối thiểu-tối đa tính toán chính xác trung vị vì chỉ có ba phần tử. Không có trạng thái trung gian nào có thể vi phạm quy tắc vì cả ba biến luôn được so sánh theo cùng một cách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

g, c, l = map(int, input().split())

mx = max(g, c, l)
mn = min(g, c, l)

if mx - mn >= 10:
    print("check again")
else:
    median = g + c + l - mx - mn
    print(f"final {median}")
```Giải pháp đọc đầu vào một cách hiệu quả và ánh xạ trực tiếp nó thành ba biến số nguyên. Việc tính toán tối đa và tối thiểu rất đơn giản và an toàn với phạm vi điểm số hạn chế. Phép tính trung vị tránh được việc sắp xếp và hiệu quả ngay cả khi hai điểm bằng nhau. Quá trình in sử dụng chuỗi f để dễ đọc và đảm bảo định dạng đầu ra khớp chính xác với những gì vấn đề mong đợi. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** Điểm`88 94 95`| g | c | tôi | mx | mn | mx-mn | trung vị | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 88 | 94 | 95 | 95 | 88 | 7 | 94 | cuối cùng 94 | 

Chênh lệch nhỏ hơn 10 nên trung vị được tính là 88+94+95-95-88=94. 

**Ví dụ 2:** Điểm số`80 90 90`| g | c | tôi | mx | mn | mx-mn | trung vị | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 80 | 90 | 90 | 90 | 80 | 10 | 90 | kiểm tra lại | 

Sự khác biệt chính xác là 10, do đó thuật toán in chính xác "kiểm tra lại" chứ không phải là số trung vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ cần ba phép so sánh để tính giá trị tối thiểu, tối đa và trung vị. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng. | 

Với các ràng buộc đầu vào, thuật toán chạy trong thời gian và không gian không đổi và dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    g, c, l = map(int, input().split())
    mx = max(g, c, l)
    mn = min(g, c, l)
    if mx - mn >= 10:
        return "check again"
    else:
        median = g + c + l - mx - mn
        return f"final {median}"

# Provided sample
assert run("88 94 95") == "final 94", "sample 1"

# Custom cases
assert run("80 90 90") == "check again", "boundary difference 10"
assert run("100 100 100") == "final 100", "all equal max"
assert run("80 80 90") == "check again", "max-min = 10"
assert run("80 85 90") == "final 85", "typical median calculation"
assert run("99 91 95") == "final 95", "unordered scores"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 80 90 90 | kiểm tra lại | Khác biệt chính xác 10 yếu tố kích hoạt đánh giá lại | 
| 100 100 100 | 100 cuối cùng | Tất cả các điểm bằng nhau đều tạo ra điểm trung bình chính xác | 
| 80 80 90 | kiểm tra lại | Kiểm tra lại hai mức tối thiểu bằng nhau và một mức kích hoạt cao | 
| 80 85 90 | cuối cùng 85 | Tính toán trung vị chính xác cho mức chênh lệch nhỏ hơn 10 | 
| 99 91 95 | cuối cùng 95 | Tính toán trung bình hoạt động cho điểm không có thứ tự | 

## Vỏ cạnh 

Trường hợp có điểm khác nhau đúng 10 được xử lý bằng phép so sánh`mx - mn >= 10`. Ví dụ, với đầu vào`80 90 90`,`mx=90`Và`mn=80`, Vì thế`mx - mn=10`, thỏa mãn điều kiện kiểm tra lại. Thuật toán không in sai số trung vị ở đây. Tương tự, khi tất cả các điểm đều bằng nhau,`mx=mn`, do đó hiệu là 0 và trung vị bằng bất kỳ điểm nào, điều này đúng. Điều này khẳng định thuật toán tôn trọng cả điều kiện bằng và sai phân biên.
