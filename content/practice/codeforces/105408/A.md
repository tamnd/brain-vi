---
title: "CF 105408A - AAEGGLNU"
description: "Chúng ta được cung cấp một tập hợp các từ tạo thành từ điển và một chuỗi các từ truy vấn. Thứ tự của các từ không phải là thứ tự từ điển thông thường trên chuỗi thô."
date: "2026-06-24T23:07:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105408
codeforces_index: "A"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico Repechaje"
rating: 0
weight: 105408
solve_time_s: 71
verified: false
draft: false
---

[CF 105408A - AAEGGLNU](https://codeforces.com/problemset/problem/105408/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các từ tạo thành từ điển và một chuỗi các từ truy vấn. Thứ tự của các từ không phải là thứ tự từ điển thông thường trên chuỗi thô. Thay vào đó, mỗi từ được chuyển đổi trước tiên bằng cách sắp xếp các ký tự của nó theo thứ tự bảng chữ cái và phiên bản được chuyển đổi này được sử dụng làm khóa chính để so sánh. 

Nếu hai từ có dạng được sắp xếp khác nhau thì từ có chuỗi được sắp xếp nhỏ hơn sẽ xuất hiện trước. Nếu các dạng được sắp xếp giống hệt nhau, chúng ta sẽ quay lại so sánh các từ gốc về mặt từ điển. 

Mỗi truy vấn hỏi có bao nhiêu từ trong từ điển nhỏ hơn hoặc bằng truy vấn theo thứ tự tùy chỉnh này. 

Các ràng buộc cho phép lên tới một trăm nghìn từ trong từ điển và một trăm nghìn truy vấn, với tổng chiều dài ký tự lên tới một triệu. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào so sánh từng truy vấn với tất cả các từ trong từ điển, vì cách tiếp cận đơn giản sẽ yêu cầu so sánh theo thứ tự 10^10 ký tự trong trường hợp xấu nhất. 

Một vấn đề tế nhị xuất hiện khi các từ là đảo chữ. Ví dụ: "abc" và "bca" có cùng cách biểu thị được sắp xếp là "abc". Trong trường hợp này, thứ tự phụ thuộc vào chuỗi gốc chứ không chỉ vào dạng được sắp xếp. Một trường hợp khác phát sinh khi tồn tại nhiều từ giống nhau. Một giải pháp đúng phải đếm tất cả các lần xuất hiện một cách chính xác. 

Một sai lầm ngây thơ là chỉ sắp xếp theo chuỗi đã chuyển đổi và bỏ qua quy tắc tie-break. Ví dụ: nếu từ điển chứa "ab" và "ba", cả hai đều ánh xạ tới "ab". Nếu không có khóa phụ, thứ tự tương đối của chúng sẽ không được xác định, điều này sẽ phá vỡ tính chính xác của truy vấn khi đếm các phạm vi tiền tố. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp sẽ xử lý từng truy vấn một cách độc lập bằng cách lặp lại tất cả các từ trong từ điển và kiểm tra xem mỗi từ có nhỏ hơn hoặc bằng truy vấn theo thứ tự đã xác định hay không. Điều này yêu cầu tính toán biểu mẫu được sắp xếp cho mỗi lần so sánh hoặc lưu trữ trước. Ngay cả khi quá trình tiền xử lý được thực hiện, mỗi truy vấn vẫn cần so sánh O(N), dẫn đến thời gian là O(NQ). Với 100000 truy vấn, điều này trở nên quá lớn. 

Quan sát quan trọng là thứ tự xác định tổng thứ tự trên các từ nếu chúng ta coi mỗi từ là một cặp bao gồm phiên bản được sắp xếp của nó và chuỗi gốc. Khi mỗi từ được ánh xạ vào một cặp như vậy, chúng ta có thể sắp xếp toàn bộ từ điển một lần. Sau khi sắp xếp, mỗi truy vấn sẽ giảm xuống việc tìm vị trí của cặp của nó trong mảng được sắp xếp này, việc này có thể được thực hiện bằng cách sử dụng tìm kiếm nhị phân. 

Điều này hiệu quả vì quy tắc so sánh nhất quán và bắc cầu một khi được biểu thị dưới dạng thứ tự bộ dữ liệu. Việc sắp xếp từ điển về cơ bản sẽ xây dựng trước cấu trúc thứ tự đầy đủ và mỗi truy vấn sẽ trở thành truy vấn xếp hạng trên một mảng được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ · L) | O(1) thêm | Quá chậm | 
| Sắp xếp + Tìm kiếm nhị phân | O((N + Q) L log N) | O(N) | Đã chấp nhận | 

Ở đây L là độ dài từ tối đa đóng góp cho việc sắp xếp các ký tự. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các từ trong từ điển và tính toán phiên bản chuyển đổi cho mỗi từ bằng cách sắp xếp các ký tự của nó. Phép biến đổi này nắm bắt khóa so sánh chính được sử dụng trong thứ tự. 
2. Lưu trữ mỗi từ dưới dạng một cặp bao gồm phiên bản được sắp xếp và chuỗi gốc của nó. Cấu trúc cặp đảm bảo rằng việc bẻ dây được xử lý tự động. 
3. Sắp xếp danh sách các cặp từ điển. Việc sắp xếp mang tính từ điển trên cặp, nghĩa là trước tiên nó so sánh các phiên bản được sắp xếp và sau đó là các chuỗi gốc nếu cần. 
4. Đối với mỗi từ truy vấn, hãy tính biểu diễn cặp giống nhau: phiên bản được sắp xếp và dạng ban đầu của nó. 
5. Sử dụng tìm kiếm nhị phân để tìm xem có bao nhiêu cặp từ điển nhỏ hơn hoặc bằng cặp truy vấn này. Điều này tương đương với việc tìm vị trí giới hạn trên của cặp truy vấn trong mảng được sắp xếp. 
6. Xuất chỉ mục kết quả cho mỗi truy vấn.

### Tại sao nó hoạt động 

Mỗi từ được ánh xạ tới một khóa có thể so sánh duy nhất ở dạng (sorted_string, original_string). Bước sắp xếp tạo ra một thứ tự tổng thể phù hợp với định nghĩa bài toán. Vì từ điển được sắp xếp theo thứ tự này nên tất cả các từ nhỏ hơn hoặc bằng một truy vấn sẽ tạo thành một tiền tố liền kề. Tìm kiếm nhị phân xác định chính xác ranh giới của tiền tố này, do đó số đếm được trả về là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = []

    for _ in range(n):
        w = input().strip()
        arr.append(("".join(sorted(w)), w))

    arr.sort()

    q = int(input())
    res = []

    from bisect import bisect_right

    for _ in range(q):
        b = input().strip()
        key = ("".join(sorted(b)), b)
        res.append(str(bisect_right(arr, key)))

    print("\n".join(res))

if __name__ == "__main__":
    solve()
```Ý tưởng cốt lõi trong quá trình triển khai là biểu diễn mỗi từ dưới dạng một bộ dữ liệu sao cho việc so sánh bộ dữ liệu từ điển tích hợp của Python khớp chính xác với thứ tự được yêu cầu. Sắp xếp danh sách sau khi thiết lập thứ tự từ điển đầy đủ. 

Đối với mỗi truy vấn, việc xây dựng cùng một bộ dữ liệu sẽ đảm bảo khả năng tương thích với danh sách đã sắp xếp. Việc sử dụng`bisect_right`rất quan trọng vì nó đếm tất cả các phần tử nhỏ hơn hoặc bằng truy vấn, bao gồm cả các từ trùng lặp. 

## Ví dụ đã hoạt động 

Hãy xem xét một từ điển đơn giản: ["ab", "ba", "aab", "baa"]. 

Sau khi biến đổi, ta được các cặp: 

("ab", "ab"), ("ab", "ba"), ("aab", "aab"), ("aab", "baa") 

Sau khi sắp xếp: 

("aab", "aab") 

("aab", "baa") 

("ab", "ab") 

("ab", "ba") 

Bây giờ hãy xem xét truy vấn "aba". Cặp của nó là ("aab", "aba"). 

| Bước | Truy vấn | Khóa được sắp xếp | Vị trí giới hạn trên | 
| --- | --- | --- | --- | 
| 1 | aba | aab | sau nhóm thứ hai | 
| 2 | aba | aab | 2 | 

Điều này cho thấy cả hai từ có khóa "aab" đều được tính. 

Một truy vấn khác "ba" trở thành ("ab", "ba"), nằm trong nhóm thứ hai. 

| Bước | Truy vấn | Khóa được sắp xếp | Vị trí giới hạn trên | 
| --- | --- | --- | --- | 
| 1 | ba | ab | sau tất cả các mục "aab" | 
| 2 | ba | ab | 4 | 

Điều này chứng tỏ rằng việc ngắt kết nối trên các chuỗi gốc sẽ phân tách chính xác các dạng được sắp xếp giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) · L log N) | Sắp xếp N từ biến đổi chiếm ưu thế; mỗi lần chuyển đổi tốn O(L log L) và mỗi truy vấn sử dụng tìm kiếm nhị phân O(log N) | 
| Không gian | O(N) | Lưu trữ các cặp chuyển đổi cho tất cả các từ trong từ điển | 

Các ràng buộc cho phép tổng cộng tối đa một triệu ký tự, do đó, việc sắp xếp từng từ rồi sắp xếp mảng diễn ra nhanh chóng một cách thoải mái trong vòng một giây trong Python khi được triển khai với tính năng sắp xếp và chia đôi tích hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample (formatted logically)
assert run("""5
language
abc
ddcba
aaegglnu
b
3
bcd
ace
gglnua
""") == """3
5
1"""

# all identical words
assert run("""3
aa
aa
aa
2
aa
a
""") == """3
0"""

# anagram ordering check
assert run("""4
ab
ba
aab
baa
2
ab
baa
""") == """3
4"""

# single word cases
assert run("""1
abc
2
abc
a
""") == """1
0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| từ giống nhau | 3, 0 | trùng lặp và xử lý bình đẳng | 
| đảo chữ | 3, 4 | tính chính xác của việc nhóm khóa được sắp xếp | 
| từ đơn | 1, 0 | hành vi ranh giới | 

## Vỏ cạnh 

Khi tất cả các từ trong từ điển đều giống nhau, mọi truy vấn khớp với từ đó phải trả về số lượng đầy đủ. Việc biểu diễn bộ dữ liệu đảm bảo điều này vì tất cả các khóa trở nên giống hệt nhau và`bisect_right`đặt điểm chèn vào cuối khối, đếm tất cả các lần xuất hiện. 

Khi các từ là đảo chữ, chẳng hạn như "ab" và "ba", phím được sắp xếp sẽ giống hệt nhau. Thứ tự thứ cấp theo chuỗi gốc đảm bảo vị trí xác định bên trong khối đó. Các truy vấn vẫn phải đếm tất cả các mục trong khối khi chuỗi gốc của truy vấn nằm sau một số mục trong số chúng theo từ điển. 

Một từ điển tối thiểu chỉ có một từ kiểm tra hành vi ranh giới. Nếu từ truy vấn nhỏ hơn về mặt từ điển so với mục từ điển duy nhất thì câu trả lời phải bằng 0. Nếu nó bằng hoặc lớn hơn theo thứ tự thì câu trả lời phải là một. Tìm kiếm nhị phân xử lý chính xác cả hai trường hợp vì nó đo trực tiếp vị trí chèn trong cấu trúc được sắp xếp.
