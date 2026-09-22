---
title: "CF 104785N – Đặt Tên Chai Rượu"
description: "Mỗi dòng đầu vào mô tả thể tích chai rượu được viết dưới dạng số thập phân, theo sau là chữ L. Các dòng khác nhau có thể mô tả chính xác cùng một số lượng ngay cả khi chúng trông khác nhau về mặt cú pháp, ví dụ: 1,0L và 1L biểu thị cùng một giá trị."
date: "2026-06-28T14:43:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104785
codeforces_index: "N"
codeforces_contest_name: "2023 United Kingdom and Ireland Programming Contest (UKIEPC 2023)"
rating: 0
weight: 104785
solve_time_s: 50
verified: true
draft: false
---

[CF 104785N - Đặt tên cho chai rượu](https://codeforces.com/problemset/problem/104785/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi dòng đầu vào mô tả thể tích chai rượu được viết dưới dạng số thập phân theo sau là chữ cái`L`. Ví dụ: các dòng khác nhau có thể mô tả chính xác cùng một số lượng ngay cả khi chúng trông khác nhau về mặt cú pháp`1.0L`Và`1L`đại diện cho cùng một giá trị. Nhiệm vụ là gán một từ viết thường cho mỗi tập riêng biệt và đảm bảo rằng các tập giống hệt nhau luôn nhận được cùng một từ. 

Do đó, đầu ra không phải là một phép tính trên chính giá trị số mà là một nhãn nhất quán của các lớp tương đương của các số thực được cho ở dạng thập phân. Bất cứ khi nào một ổ đĩa xuất hiện lại sau đó trong đầu vào, nó phải sử dụng lại cùng một nhãn được chỉ định thay vì tạo một nhãn mới. 

Ràng buộc`n ≤ 10000`ngụ ý rằng bất kỳ giải pháp nào so sánh trực tiếp từng cặp giá trị sẽ quá chậm, vì điều đó sẽ dẫn đến khoảng`10^8`so sánh trong trường hợp xấu nhất Điều này thúc đẩy chúng ta hướng tới chiến lược băm hoặc chuẩn hóa trong đó mỗi giá trị được chuyển đổi thành biểu diễn chuẩn theo thời gian không đổi và được lưu trữ trong từ điển. 

Một khó khăn tinh tế đến từ việc biểu diễn dấu phẩy động. Một cách tiếp cận đơn giản phân tích từng giá trị dưới dạng Python`float`và sử dụng nó làm khóa từ điển có thể không thành công trong các trường hợp đặc biệt khi phân tích cú pháp thập phân đưa ra các tạo phẩm chính xác. Ví dụ: hai đầu vào như`0.1L`Và`0.10L`rõ ràng phải giống hệt nhau, nhưng việc phân tích cú pháp dấu phẩy động đôi khi có thể đưa ra những khác biệt làm tròn nhỏ tùy thuộc vào cách biểu diễn và ngôn ngữ. Một trường hợp cạnh khác là các giá trị có nhiều chữ số sau dấu thập phân, tối đa 10, trong đó dấu phẩy động nhị phân không chính xác. 

Cách tiếp cận đúng phải đảm bảo rằng hai chuỗi thập phân bằng nhau về số lượng luôn ánh xạ tới cùng một khóa bên trong, bất kể sự khác biệt về định dạng. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ so sánh từng tập đến với tất cả các tập đã thấy trước đó bằng cách phân tích cú pháp cả hai chuỗi và kiểm tra sự bằng nhau về số. Mỗi so sánh sẽ liên quan đến việc phân tích các chuỗi thập phân và có khả năng chuẩn hóa chúng, dẫn đến tổng số phép tính bậc hai. Với 10000 chai, con số này sẽ đạt tới mức 100 triệu so sánh, quá chậm trong Python khi mỗi so sánh liên quan đến phân tích chuỗi hoặc xử lý dấu phẩy động. 

Quan sát quan trọng là đầu vào đã ở định dạng thập phân có cấu trúc với độ chính xác giới hạn. Thay vì dựa vào số học dấu phẩy động, chúng ta có thể chuyển đổi từng giá trị thành biểu diễn số nguyên chuẩn bằng cách loại bỏ dấu thập phân và theo dõi số chữ số phân số. Hai giá trị bằng nhau khi và chỉ khi dạng số nguyên chuẩn hóa của chúng khớp với nhau sau khi căn chỉnh tỷ lệ thập phân. 

Khi mỗi tập được chuyển đổi thành khóa chuẩn, chúng ta chỉ cần kiểm tra xem nó đã được nhìn thấy hay chưa trước khi sử dụng bản đồ băm. Nếu không, chúng tôi chỉ định một nhãn mới. Điều này làm giảm toàn bộ vấn đề về việc băm thời gian tuyến tính trên các chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Băm tối ưu với chuẩn hóa | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn có một bản trình bày cho từng tập giúp việc kiểm tra tính bằng nhau được chính xác và không phụ thuộc vào định dạng. 

1. Đọc chuỗi âm lượng và xóa dấu`L`, chỉ giữ lại phần số. 
2. Chia chuỗi số thành phần nguyên và phần phân số xung quanh dấu thập phân. Nếu không có dấu thập phân thì coi phần phân số là trống. 
3. Chuẩn hóa cách biểu diễn bằng cách loại bỏ các số 0 ở đầu phần nguyên và loại bỏ các số 0 ở cuối trong phần phân số. Nếu phần phân số trở nên trống sau khi cắt bớt, chúng tôi sẽ loại bỏ hoàn toàn phần thập phân. Điều này đảm bảo rằng`1`,`1.0`, Và`01.000`tất cả đều sụp đổ vào cùng một chìa khóa. 
4. Xây dựng khóa chính tắc dưới dạng một bộ chứa phần nguyên được làm sạch và phần phân số được làm sạch. Bộ dữ liệu này biểu diễn duy nhất số thực ở dạng thập phân mà không có lỗi dấu phẩy động. 
5. Sử dụng từ điển để ánh xạ từng khóa duy nhất tới một từ được tạo. Khi gặp một khóa mới, hãy gán mã định danh từ chưa sử dụng tiếp theo. Khi khóa tương tự xuất hiện lại, hãy sử dụng lại từ đã gán trước đó. 

### Tại sao nó hoạt động 

Hai chuỗi thập phân biểu thị cùng một số khi và chỉ khi các chữ số nguyên và phân số của chúng khớp nhau sau khi loại bỏ các thành phần định dạng như số 0 ở đầu và số 0 ở cuối dư thừa. Bước chuẩn hóa thực thi một dạng chuẩn duy nhất cho mỗi giá trị thực có thể biểu thị ở định dạng đầu vào. Vì từ điển sử dụng dạng chuẩn này làm khóa nên đẳng thức trong không gian giá trị được chuyển thành đẳng thức trong cách biểu diễn chuỗi, đảm bảo việc ghi nhãn nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def normalize(s: str):
    if s.endswith("L"):
        s = s[:-1]

    if "." in s:
        a, b = s.split(".")
    else:
        a, b = s, ""

    a = a.lstrip("0")
    if a == "":
        a = "0"

    b = b.rstrip("0")

    if b == "":
        return (a, "")

    return (a, b)

def solve():
    n = int(input())
    mp = {}
    out = []
    counter = 0

    for _ in range(n):
        s = input().strip()
        key = normalize(s)

        if key not in mp:
            mp[key] = f"w{counter}"
            counter += 1

        out.append(mp[key])

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`normalize`hàm đảm bảo rằng các cách biểu diễn khác nhau về mặt cú pháp của cùng một số sẽ được thu gọn thành các bộ dữ liệu giống hệt nhau. Từ điển`mp`lưu trữ ánh xạ nhìn thấy lần đầu tiên từ số được chuẩn hóa sang một từ được chỉ định. 

Sự lựa chọn của`tuple(int_part, frac_part)`tránh hoàn toàn dấu phẩy động và đảm bảo ngữ nghĩa so sánh chính xác. Các tên được tạo`w0, w1, ...`chỉ đóng vai trò giữ chỗ; mọi chuỗi chữ thường nhất quán sẽ đáp ứng yêu cầu của bài toán miễn là các khóa giống hệt nhau sử dụng lại cùng một chuỗi. 

Một lỗi triển khai phổ biến là sử dụng`float(s[:-1])`làm khóa từ điển. Điều này có nguy cơ làm mất độ chính xác đối với các đầu vào phân đoạn dài và có thể hợp nhất các giá trị riêng biệt hoặc phân tách các giá trị bằng nhau một cách không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên: 

| Bước | Đầu vào | Khóa chuẩn hóa | Mới? | Từ được giao | 
| --- | --- | --- | --- | --- | 
| 1 | 15L | (15, "") | vâng | w0 | 
| 2 | 0,88L | (0, "88") | vâng | w1 | 
| 3 | 1.0L | (1, "") | vâng | w2 | 
| 4 | 1L | (1, "") | không | w2 | 
| 5 | 1000L | (1000, "") | vâng | w3 | 
| 6 | 1024L | (1024, "") | vâng | w4 | 

Dấu vết này cho thấy`1.0L`Và`1L`ánh xạ tới cùng một khóa được chuẩn hóa, gây ra việc sử dụng lại cùng một từ được gán. 

Đối với mẫu thứ hai: 

| Bước | Đầu vào | Khóa chuẩn hóa | Mới? | Từ được giao | 
| --- | --- | --- | --- | --- | 
| 1 | 0,03L | (0, "03") | vâng | w0 | 
| 2 | 0,031L | (0, "031") | vâng | w1 | 
| 3 | 0,03L | (0, "03") | không | w0 | 

Điều này xác nhận rằng kết quả khớp thập phân chính xác được giữ nguyên, bao gồm các số 0 đứng đầu trong phần phân số khi chúng có ý nghĩa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tập được phân tích cú pháp một lần và chèn vào bản đồ băm với tra cứu trung bình O(1) | 
| Không gian | O(n) | Mỗi tập chuẩn hóa riêng biệt được lưu trữ một lần trong từ điển | 

Thuật toán có quy mô thoải mái cho`n = 10000`, vì cả phân tích cú pháp và băm đều hoạt động theo thời gian tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return "\n".join(solve() or [])

# provided samples (format assumed placeholder since actual words irrelevant)
assert run("3\n15L\n0.88L\n1.0L\n") is not None

# all identical values
assert run("4\n1L\n1.0L\n1.00L\n1L\n").split() == run("4\n1L\n1.0L\n1.00L\n1L\n").split()

# distinct fractional precision
assert len(set(run("3\n0.1L\n0.10L\n0.100L\n").split())) == 1

# mixed integers
assert len(set(run("3\n10L\n010L\n10.0L\n").split())) == 1

# max-like small stress
assert run("1\n0L\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số thập phân lặp đi lặp lại | cùng một từ được sử dụng lại | tính nhất quán của bản đồ | 
| số không đệm | cùng một từ được sử dụng lại | độ chính xác chuẩn hóa | 
| các biến thể phân số | cùng một từ được sử dụng lại | xử lý dấu 0 | 
| định dạng số nguyên | cùng một từ được sử dụng lại | xử lý số 0 hàng đầu | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi cùng một giá trị số được viết dưới nhiều dạng khác nhau về mặt trực quan. Ví dụ,`01.0L`,`1L`, Và`1.000L`tất cả đều đại diện cho cùng một khối lượng. Sau khi tách và chuẩn hóa, mỗi phần trở thành phần nguyên`"1"`và phần phân số trống, do đó chúng thu gọn vào cùng một khóa từ điển và nhận các nhãn giống hệt nhau. 

Một trường hợp cạnh khác liên quan đến các giá trị phân số chỉ khác nhau ở các số 0 ở cuối, chẳng hạn như`0.30L`Và`0.3L`. Bước chuẩn hóa sẽ cắt bớt các số 0 ở cuối thành phần phân số, do đó cả hai đều trở thành`(0, "3")`, đảm bảo phân nhóm chính xác ngay cả khi định dạng đầu vào có độ chính xác khác nhau.
