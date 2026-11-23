---
title: "CF 59A - Từ"
description: "Nhiệm vụ là bình thường hóa kiểu chữ của một từ sao cho tất cả các chữ cái đều là chữ thường hoặc tất cả đều là chữ hoa."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 59
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 55 (Div. 2)"
rating: 800
weight: 59
solve_time_s: 71
verified: true
draft: false
---

[CF 59A - Từ](https://codeforces.com/problemset/problem/59/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là bình thường hóa kiểu chữ của một từ sao cho tất cả các chữ cái đều là chữ thường hoặc tất cả đều là chữ hoa. Quyết định phụ thuộc vào trường hợp nào chiếm ưu thế trong đầu vào: nếu có nhiều chữ hoa hơn chữ thường, từ đó sẽ được chuyển hoàn toàn thành chữ hoa; nếu không, nó phải được chuyển đổi hoàn toàn thành chữ thường. Nếu số lượng bằng nhau thì ưu tiên sử dụng chữ thường. 

Đầu vào là một từ duy nhất chỉ chứa các chữ cái tiếng Anh, có độ dài từ 1 đến 100 ký tự. Độ dài này nhỏ nên chúng ta có thể lặp lại tất cả các ký tự nhiều lần mà không cần lo lắng về hiệu quả. Không cần cấu trúc dữ liệu phức tạp hoặc sắp xếp. 

Các trường hợp cạnh bao gồm các từ đã có kiểu chữ giống nhau, một từ có một chữ cái và các từ có số lượng chữ hoa và chữ thường hoàn toàn bằng nhau. Ví dụ, đầu vào`"a"`nên ở lại`"a"`, Và`"Ab"`nên trở thành`"ab"`bởi vì các ràng buộc chữ thường bị phá vỡ có lợi cho chữ thường. Việc triển khai ngây thơ có thể quên xử lý chính xác điều kiện ràng buộc hoặc có thể đếm sai các chữ cái do không phân biệt rõ ràng giữa chữ hoa và chữ thường. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ quét chuỗi và đối với mỗi chữ cái, hãy kiểm tra xem nó là chữ hoa hay chữ thường và duy trì các bộ đếm riêng biệt. Sau đó, sau khi đếm, so sánh tổng số và chuyển đổi từ cho phù hợp. Cách tiếp cận này thực sự đủ hiệu quả đối với các ràng buộc của chúng tôi, vì từ này có tối đa 100 ký tự và mỗi thao tác là O(1). Vì vậy, ngay cả việc quét hai lần - một để đếm và một để chuyển đổi - cũng có thể chấp nhận được. 

Quan sát quan trọng giúp đơn giản hóa việc triển khai là Python cung cấp các phương thức chuỗi tích hợp để kiểm tra trường hợp (`isupper()`) và chuyển đổi toàn bộ chuỗi thành một trường hợp cụ thể (`upper()`Và`lower()`). Chúng tôi không cần theo dõi các chỉ mục riêng lẻ để chuyển đổi vì chuyển đổi áp dụng cho toàn bộ chuỗi. 

Sự khác biệt giữa brute-force và tối ưu ở đây chủ yếu là về mặt phong cách. Cách tiếp cận tối ưu chỉ đơn giản là quét tuyến tính một lần để đếm các chữ cái viết hoa, sau đó áp dụng có điều kiện`upper()`hoặc`lower()`đến toàn bộ chuỗi. Không cần một cấu trúc phức tạp hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đếm + chuyển đổi) | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (đếm chữ hoa, chuyển đổi một lần) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm cho chữ hoa. Đây là trạng thái tối thiểu cần thiết để quyết định sự chuyển đổi cuối cùng. 
2. Lặp lại từng ký tự trong từ đầu vào. Đối với mỗi ký tự, hãy kiểm tra xem nó có phải là chữ hoa hay không bằng phương pháp ngôn ngữ chuẩn. Tăng bộ đếm chữ hoa nếu có. Chúng ta không cần bộ đếm chữ thường vì tổng chiều dài trừ đi số chữ hoa sẽ cho ra số lượng chữ thường. 
3. Sau khi quét tất cả các chữ cái, so sánh số lượng chữ hoa với số lượng chữ thường (được tính bằng tổng chiều dài trừ đi số lượng chữ hoa). Nếu số lượng chữ hoa lớn hơn nhiều, hãy chuyển toàn bộ từ thành chữ hoa. Ngược lại, chuyển toàn bộ từ sang chữ thường. 
4. In từ đã chuyển đổi. 

Tại sao nó hoạt động: ở bước 3, việc so sánh đảm bảo rằng số lần chuyển đổi chữ cái tối thiểu được áp dụng. Nếu chữ hoa chiếm ưu thế, việc chuyển đổi tất cả thành chữ hoa sẽ thay đổi ít chữ cái hơn so với chuyển đổi sang chữ thường. Nếu chữ thường chiếm ưu thế hoặc bằng nhau, việc chuyển đổi sang chữ thường sẽ đạt được mục tiêu thay đổi tối thiểu tương tự. Điều bất biến là tại thời điểm chuyển đổi, chúng ta đã biết trường hợp đa số toàn cục nên không thể xảy ra tính toán sai cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

word = input().strip()

uppercase_count = sum(1 for c in word if c.isupper())
lowercase_count = len(word) - uppercase_count

if uppercase_count > lowercase_count:
    print(word.upper())
else:
    print(word.lower())
```Dòng đầu tiên đọc đầu vào một cách hiệu quả. sử dụng`strip()`loại bỏ bất kỳ dòng mới nào ở cuối. Sự hiểu biết`sum(1 for c in word if c.isupper())`đếm các chữ cái viết hoa trong một lượt. Số lượng chữ thường được tính là chênh lệch giữa tổng chiều dài và số lượng chữ hoa, tránh lặp lại lần thứ hai. Điều kiện áp dụng phép biến đổi chính xác trong một bước. 

Điểm tinh tế: quên`strip()`sẽ để lại một dòng mới ở đầu ra. Đếm nhầm chữ hoa và chữ thường hoặc sử dụng`>=`thay vì`>`sẽ vi phạm nguyên tắc hòa giải. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`"HoUse"`| Bước | Số chữ hoa | Số chữ thường | Quyết định | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đếm chữ cái | 2 (`H`,`U`) | 3 (`o`,`s`,`e`) | chữ thường chiếm ưu thế |`house`| 

Điều này xác nhận thuật toán chọn đúng chữ thường khi nó chiếm đa số. 

### Ví dụ 2 

đầu vào:`"ViP"`| Bước | Số chữ hoa | Số chữ thường | Quyết định | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đếm chữ cái | 2 (`V`,`P`) | 1 (`i`) | chữ hoa chiếm ưu thế |`VIP`| 

Điều này chứng tỏ khả năng xử lý đúng khi chữ in hoa xuất hiện thường xuyên hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét chuỗi một lần để đếm và sau đó áp dụng một thao tác chuyển đổi duy nhất. Mỗi bước có độ dài từ tuyến tính. | 
| Không gian | O(1) | Chỉ có hai bộ đếm và một vài tài liệu tham khảo tạm thời được sử dụng. Chuỗi đầu ra chia sẻ bộ nhớ với đầu vào khi triển khai Python. | 

Với n 100, giải pháp này chạy ngay lập tức và yêu cầu bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    word = sys.stdin.readline().strip()
    uppercase_count = sum(1 for c in word if c.isupper())
    lowercase_count = len(word) - uppercase_count
    return word.upper() if uppercase_count > lowercase_count else word.lower()

# provided sample
assert run("HoUse\n") == "house", "sample 1"
# all lowercase
assert run("abcde\n") == "abcde", "all lowercase"
# all uppercase
assert run("ABCDE\n") == "ABCDE", "all uppercase"
# tie case
assert run("Ab\n") == "ab", "tie case -> lowercase"
# single character uppercase
assert run("X\n") == "X", "single uppercase"
# single character lowercase
assert run("x\n") == "x", "single lowercase"
# mixed with more uppercase
assert run("PyTHon\n") == "PYTHON", "more uppercase"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| HoUse | nhà | chữ thường đa số | 
| abcde | abcde | tất cả đầu vào chữ thường | 
| ABCDE | ABCDE | tất cả đầu vào chữ hoa | 
| Ab | ab | cà vạt được xử lý đúng cách | 
| X | X | chữ hoa một chữ cái | 
| x | x | chữ thường một chữ cái | 
| PyTHon | TRĂN | thêm chữ in hoa | 

## Vỏ cạnh 

Đối với kịch bản hòa`"Ab"`, thuật toán tính toán`uppercase_count = 1`,`lowercase_count = 1`. điều kiện`uppercase_count > lowercase_count`không thành công nên từ đó được chuyển thành chữ thường`"ab"`. Điều này áp dụng chính xác quy tắc ràng buộc chữ thường. 

Đối với một từ có một chữ cái`"X"`, số lượng chữ hoa là 1, số lượng chữ thường là 0. Điều kiện là đúng nên đầu ra vẫn giữ nguyên`"X"`. Vì`"x"`, điều kiện là sai và đầu ra vẫn giữ nguyên`"x"`. Cả hai trường hợp đều xác nhận thuật toán xử lý chính xác kích thước đầu vào tối thiểu. 

Bài xã luận này đảm bảo rằng người đọc có thể tái tạo cách tiếp cận đối với bất kỳ vấn đề tương tự nào về chuẩn hóa trường hợp hoặc chuyển đổi dựa trên đa số.
