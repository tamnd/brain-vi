---
title: "CF 130B - Gnikool Ssalg"
description: "Vấn đề yêu cầu chúng ta đảo ngược một chuỗi. Cho một chuỗi ký tự, đầu ra phải có cùng một chuỗi nhưng theo thứ tự ngược lại, sao cho ký tự đầu tiên trở thành ký tự cuối cùng, ký tự thứ hai trở thành ký tự thứ hai đến cuối cùng, v.v."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 130
codeforces_index: "B"
codeforces_contest_name: "Unknown Language Round 4"
rating: 1400
weight: 130
solve_time_s: 84
verified: true
draft: false
---

[CF 130B - Gnikool Ssalg](https://codeforces.com/problemset/problem/130/B) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, triển khai, chuỗi 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề yêu cầu chúng ta đảo ngược một chuỗi. Cho một chuỗi ký tự, đầu ra phải có cùng một chuỗi nhưng theo thứ tự ngược lại, sao cho ký tự đầu tiên trở thành ký tự cuối cùng, ký tự thứ hai trở thành ký tự thứ hai đến cuối cùng, v.v. Chuỗi đầu vào được đảm bảo có từ 1 đến 100 ký tự và mọi ký tự đều nằm trong phạm vi ASCII có thể in được từ 33 (dấu chấm than) đến 126 (dấu ngã). 

Những hạn chế là nhẹ. Với tối đa 100 ký tự và giới hạn thời gian 2 giây, bất kỳ thuật toán hợp lý nào xử lý mỗi ký tự với số lần không đổi sẽ chạy thoải mái. Chúng ta không cần phải lo lắng về các thuật toán có độ phức tạp trên O(n), vì n nhỏ. Không có lo ngại về tràn số nguyên hoặc vấn đề về độ chính xác vì đầu vào hoàn toàn là văn bản. 

Các trường hợp khó khăn ở đây rất tinh tế. Một chuỗi có một ký tự sẽ tự xuất ra, ví dụ như đầu vào`A`phải quay lại`A`. Một chuỗi trong đó tất cả các ký tự đều giống hệt nhau, như`!!!`, cũng sẽ trả về cùng một chuỗi. Việc xử lý các chuỗi trống là không cần thiết vì độ dài đầu vào được đảm bảo ít nhất là 1. Việc triển khai cẩn thận phải đảm bảo rằng không có ký tự nào bị bỏ hoặc sắp xếp lại không chính xác do lỗi từng ký tự. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là xây dựng một chuỗi mới theo cách thủ công bằng cách lặp lại chuỗi gốc từ đầu đến cuối và nối từng ký tự vào chuỗi kết quả mới. Điều này đúng vì nó tái tạo lại từng ký tự của chuỗi đảo ngược theo đúng nghĩa đen. Mặc dù nó liên quan đến việc tạo một chuỗi mới, nhưng tối đa 100 lần lặp sẽ đảm bảo chi phí hoạt động là không đáng kể. Trong Python, việc nối các chuỗi lặp đi lặp lại hơi kém hiệu quả, nhưng ở đây điều đó không phải là vấn đề đáng lo ngại. 

Cách tiếp cận tối ưu tận dụng khả năng ngôn ngữ tích hợp. Chuỗi Python hỗ trợ cắt bằng đối số bước, cho phép chúng ta đảo ngược chuỗi trong một biểu thức duy nhất, có thể đọc được:`s[::-1]`. Cách tiếp cận này có độ phức tạp về mặt lý thuyết tương tự như phương pháp brute-force nhưng tránh các vòng lặp thủ công, giảm nguy cơ mắc lỗi từng lỗi một và làm cho mã sạch hơn. Kích thước đầu vào nhỏ của vấn đề có nghĩa là chúng ta không bị hạn chế bởi bộ nhớ hoặc thời gian thực thi, vì vậy phương pháp này hoàn toàn có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Cắt lát | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào từ đầu vào tiêu chuẩn. Điều này ghi lại chuỗi ký tự chúng ta cần đảo ngược. Chúng tôi loại bỏ mọi dòng mới ở cuối để tránh nó xuất hiện ở đầu ra. 
2. Đảo ngược chuỗi. Sử dụng Python cắt`s[::-1]`duyệt chuỗi từ ký tự cuối cùng đến ký tự đầu tiên, tạo ra một chuỗi mới có các ký tự theo thứ tự ngược lại. 
3. In chuỗi đảo ngược. Đầu ra phải khớp chính xác với chuỗi dự kiến, bao gồm tất cả các ký tự theo thứ tự mới. 

Tại sao nó hoạt động: thao tác cắt đảm bảo rằng mỗi ký tự từ chuỗi gốc xuất hiện chính xác một lần ở đầu ra, nhưng ở vị trí ngược lại so với đầu và cuối. Không có ký tự nào bị mất, trùng lặp hoặc sắp xếp lại không chính xác. Điều này bảo toàn tính bất biến cơ bản: ký tự thứ i từ đầu chuỗi đảo ngược là ký tự thứ i tính từ cuối chuỗi gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().rstrip()
print(s[::-1])
```Giải pháp đọc chuỗi, xóa mọi dòng mới ở cuối bằng cách sử dụng`rstrip()`, đảo ngược nó bằng cách cắt và in kết quả. Việc loại bỏ dòng mới là tinh tế nhưng cần thiết vì nếu không`input()`có thể bao gồm một`\n`ở cuối, ký tự này sẽ xuất hiện dưới dạng ký tự không mong đợi ở đầu ra bị đảo ngược. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`secrofedoc`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào | s | 'secrofedoc' | 
| Đảo ngược | s[::-1] | 'lực lượng mật mã' | 
| In | đầu ra | 'lực lượng mật mã' | 

Dấu vết này xác nhận rằng mỗi ký tự được định vị lại chính xác từ cuối đến đầu. 

**Mẫu 2** 

đầu vào:`A`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào | s | 'A' | 
| Đảo ngược | s[::-1] | 'A' | 
| In | đầu ra | 'A' | 

Điều này thể hiện việc xử lý đầu vào có độ dài tối thiểu. Ký tự đơn không thay đổi, xác nhận việc xử lý trường hợp cạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập chính xác một lần trong quá trình cắt, trong đó n là độ dài chuỗi. | 
| Không gian | O(n) | Chuỗi đảo ngược được lưu trữ trong một chuỗi mới có cùng độ dài với đầu vào. | 

Với n ≤ 100, những độ phức tạp này không đáng kể để xử lý trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 64 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().rstrip()
    return s[::-1]

# provided samples
assert run("secrofedoc\n") == "codeforces", "sample 1"

# custom cases
assert run("A\n") == "A", "single character"
assert run("!!!\n") == "!!!", "all identical characters"
assert run("abcdefghijklmnopqrstuvwxyz\n") == "zyxwvutsrqponmlkjihgfedcba", "ordered alphabet"
assert run("1234567890\n") == "0987654321", "numeric characters"
assert run("~!@#$%^&*()\n") == ")(*&^%$#@!~", "special ASCII characters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| A | A | đầu vào có độ dài tối thiểu | 
| !!! | !!! | tất cả các ký tự giống hệt nhau | 
| abcdefghijklmnopqrstuvwxyz | zyxwvutsrqponmlkjihgfedcba | chuỗi dài các ký tự riêng biệt | 
| 1234567890 | 0987654321 | dãy số | 
| ~!@#$%^&*() | )(*&^%$#@!~ | xử lý ký tự ASCII đặc biệt | 

## Vỏ cạnh 

Đối với đầu vào`A`, thuật toán sẽ đọc ký tự đơn, đảo ngược nó (gần như không thay đổi) và in nó. Đối với đầu vào`!!!`, mỗi ký tự được truy cập theo thứ tự ngược lại, nhưng vì tất cả đều giống hệt nhau nên đầu ra vẫn giữ nguyên`!!!`. Cả hai trường hợp đều cho thấy phương pháp cắt lát xử lý độ dài tối thiểu và các ký tự lặp lại mà không gặp lỗi. Việc theo dõi từng bước xác nhận rằng logic đảo ngược bảo toàn danh tính của từng ký tự và vị trí chính xác tương ứng với điểm bắt đầu và kết thúc của chuỗi.
