---
title: "CF 105791A - Tự động hoàn thành"
description: "Chúng ta được cung cấp một từ điển cố định gồm các từ và sau đó là một chuỗi các chuỗi truy vấn. Đối với mỗi truy vấn, chúng tôi muốn biết có bao nhiêu từ trong từ điển bắt đầu bằng truy vấn đó làm tiền tố."
date: "2026-06-21T14:54:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105791
codeforces_index: "A"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2025"
rating: 0
weight: 105791
solve_time_s: 54
verified: true
draft: false
---

[CF 105791A - Tự động hoàn thành](https://codeforces.com/problemset/problem/105791/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một từ điển cố định gồm các từ và sau đó là một chuỗi các chuỗi truy vấn. Đối với mỗi truy vấn, chúng tôi muốn biết có bao nhiêu từ trong từ điển bắt đầu bằng truy vấn đó làm tiền tố. Một từ được tính nếu nó có chuỗi truy vấn làm phân đoạn bắt đầu, ngay cả khi nó hoàn toàn bằng truy vấn hoặc dài hơn truy vấn đó. 

Kích thước đầu vào cho thấy rõ rằng cả từ điển và danh sách truy vấn đều có thể chứa tới một trăm nghìn chuỗi và tổng số ký tự trên tất cả các chuỗi được giới hạn bởi một triệu. Điều này loại trừ mọi cách tiếp cận liên tục quét toàn bộ từ điển cho mỗi truy vấn. Một kiểm tra đơn giản so sánh mọi truy vấn với từng ký tự từ theo ký tự sẽ yêu cầu tối đa khoảng$10^{10}$so sánh trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong một giây trong Python. 

Một hạn chế tinh tế hơn đến từ việc phân bổ độ dài chuỗi. Ngay cả khi các chuỗi riêng lẻ ngắn, số lượng so sánh vẫn tăng theo$N \cdot Q$, đó là nút cổ chai chi phối. Bất kỳ giải pháp nào được chấp nhận đều phải giảm mỗi truy vấn xuống mức tối đa là công việc logarit hoặc thời gian không đổi sau khi xử lý trước. 

Một số tình huống cận biên quan trọng đối với tính đúng đắn. Nếu chuỗi truy vấn dài hơn tất cả các từ trong từ điển thì câu trả lời phải bằng 0. Ví dụ: nếu từ điển chứa "a", "ab", "abc" và truy vấn là "abcd", câu trả lời là 0 vì không từ nào có thể khớp với tiền tố đó. Một trường hợp khác là khi có nhiều từ giống nhau trong từ điển. Mỗi lần xuất hiện phải được tính riêng vì bài toán yêu cầu số lượng từ chứ không phải các từ riêng biệt. Cuối cùng, tiền tố trống không có trong đầu vào nên chúng ta không cần xử lý trường hợp đó một cách rõ ràng. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là xử lý từng truy vấn bằng cách quét từng từ trong từ điển và kiểm tra xem nó có bắt đầu bằng truy vấn hay không. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa của tiền tố. Vấn đề là chi phí. Mỗi lần kiểm tra có thể yêu cầu so sánh với toàn bộ độ dài của truy vấn, do đó chi phí của một truy vấn$O(N \cdot L)$và trên tất cả các truy vấn, điều này trở nên quá chậm. 

Quan sát chính là các truy vấn tiền tố trở nên dễ dàng nếu từ điển được sắp xếp theo từ điển. Theo thứ tự được sắp xếp, tất cả các từ có chung tiền tố sẽ tạo thành một khối liền kề. Điều này xảy ra vì thứ tự từ điển nhóm các chuỗi theo ký tự khác nhau sớm nhất của chúng. Sau khi được sắp xếp, việc trả lời truy vấn sẽ giảm xuống việc tìm vị trí đầu tiên nơi một từ có tiền tố đó có thể xuất hiện và vị trí đầu tiên mà các từ không khớp với tiền tố đó. 

Điều này làm giảm vấn đề thành hai tìm kiếm nhị phân. Một tìm kiếm tìm ranh giới bên trái, từ đầu tiên không nhỏ hơn tiền tố. Tìm kiếm thứ hai tìm thấy ranh giới phù hợp, có thể được mô phỏng bằng cách tìm kiếm chuỗi nhỏ nhất lớn hơn bất kỳ chuỗi nào bắt đầu bằng tiền tố. Một thủ thuật tiêu chuẩn là thêm một ký tự lớn hơn 'z' vào tiền tố, chẳng hạn như '{', vì thứ tự ASCII đảm bảo tất cả các chuỗi chữ thường đều đứng trước nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot Q \cdot L)$|$O(1)$thêm | Quá chậm | 
| Đã sắp xếp + Tìm kiếm nhị phân |$O((N+Q)\log N)$|$O(1)$thêm ngoài dung lượng lưu trữ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào ranh giới sắp xếp và tìm kiếm nhị phân trên các chuỗi được sắp xếp theo từ điển. 

1. Đọc tất cả các từ trong từ điển và sắp xếp chúng theo từ điển. Điều này nhóm tất cả các từ có cùng tiền tố thành các phân đoạn liền kề, đó là điều giúp cho việc tính phạm vi có thể thực hiện được. 
2. Đối với mỗi chuỗi truy vấn, hãy tính ranh giới bên trái bằng tìm kiếm nhị phân. Điều này tìm thấy chỉ mục đầu tiên trong mảng được sắp xếp trong đó một từ không nhỏ hơn chuỗi truy vấn. 
3. Xây dựng chuỗi trợ giúp bằng cách thêm một ký tự lớn hơn bất kỳ chữ cái viết thường nào, ví dụ: '{', vào truy vấn. Điều này xác định giới hạn trên cho tất cả các từ có chung tiền tố. 
4. Tính ranh giới bên phải bằng cách sử dụng tìm kiếm nhị phân trên chuỗi đã sửa đổi này. Điều này tìm thấy chỉ mục đầu tiên trong đó một từ không nhỏ hơn nhóm từ điển tiếp theo sau tất cả các từ bắt đầu bằng tiền tố. 
5. Câu trả lời cho câu hỏi là sự khác biệt giữa ranh giới bên phải và ranh giới bên trái. Sự khác biệt này tính chính xác số lượng từ trong từ điển có tiền tố phù hợp với truy vấn. 

### Tại sao nó hoạt động 

Việc sắp xếp đảm bảo rằng tất cả các chuỗi có tiền tố chung chiếm một khoảng liên tục theo thứ tự từ điển. Ranh giới bên trái cô lập ứng cử viên đầu tiên có thể khớp với tiền tố. Chuỗi giới hạn trên nhân tạo đảm bảo rằng mọi chuỗi bắt đầu bằng tiền tố hoàn toàn nhỏ hơn chuỗi đó, do đó ranh giới bên phải dừng ngay sau chuỗi khớp cuối cùng. Do đó, phép trừ tính chính xác kích thước của khối liền kề đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    words = [input().strip() for _ in range(n)]
    words.sort()

    import bisect

    out = []
    for _ in range(q):
        s = input().strip()
        left = bisect.bisect_left(words, s)
        right = bisect.bisect_left(words, s + '{')
        out.append(str(right - left))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai cốt lõi xoay quanh việc so sánh chuỗi từ điển của Python. Sắp xếp danh sách một lần đảm bảo rằng tìm kiếm nhị phân hoạt động nhất quán theo thứ tự chuỗi. chức năng`bisect_left(words, s)`tìm thấy vị trí đầu tiên ở đó`s`có thể được chèn vào mà không phá vỡ trật tự, tương ứng chính xác với từ đầu tiên không nhỏ hơn`s`. 

Ranh giới thứ hai sử dụng`s + '{'`, lớn hơn bất kỳ chuỗi chữ thường nào bắt đầu bằng`s`. Điều này tránh phải tính toán một hàm tiền tố tiếp theo phức tạp. Một sai lầm phổ biến là cố gắng`s + 'z'`, không thành công khi tiền tố đã kết thúc bằng 'z'. Sử dụng '{' sẽ tránh được tất cả các trường hợp cạnh như vậy một cách rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
4 4
a
ab
abc
abcd
a
ab
abc
b
```Sau khi sắp xếp, từ điển vẫn được giữ nguyên. 

Đối với truy vấn "a", ranh giới bên trái là chỉ số 0 và ranh giới bên phải cho "a{" là chỉ mục 4, do đó kết quả là 4. 

| Truy vấn | Trái | Đúng | Kết quả | 
| --- | --- | --- | --- | 
| một | 0 | 4 | 4 | 
| ab | 1 | 4 | 3 | 
| abc | 2 | 4 | 2 | 
| b | 4 | 4 | 0 | 

Dấu vết này cho thấy cách mỗi tiền tố xác định một phân đoạn thu nhỏ của cùng một khối liền kề. 

Bây giờ hãy xem xét:```
5 3
dog
deer
deal
cat
car
de
ca
z
```Sau khi sắp xếp:```
car, cat, deal, deer, dog
```Đối với "de", trái là 2 và phải là 4, cho 2. Đối với "ca", trái là 0 và phải là 2, cho 2. Đối với "z", cả hai ranh giới đều là 5, cho 0. 

Điều này xác nhận rằng các tiền tố không tồn tại ánh xạ chính xác tới các phạm vi trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N+Q)\log N)$| Sắp xếp chiếm ưu thế với$N \log N$, mỗi truy vấn sử dụng hai tìm kiếm nhị phân | 
| Không gian |$O(1)$thêm | Chỉ lưu trữ mảng đầu vào ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép lên đến$10^5$chuỗi và truy vấn cũng như sắp xếp cộng với tìm kiếm nhị phân phù hợp thoải mái trong giới hạn thời gian. Tổng giới hạn ký tự là một triệu đảm bảo rằng chi phí xử lý chuỗi vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    old = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# sample-like test
assert run("""4 4
a
ab
abc
abcd
a
ab
abc
b
""") == "4\n3\n2\n0"

# single element exact match
assert run("""1 2
aaaa
a
aaaa
""") == "1\n1"

# no matches
assert run("""3 2
dog
cat
mouse
z
doz
""") == "0\n0"

# identical words
assert run("""4 1
a
a
a
a
a
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| từ đơn lặp đi lặp lại | 1/1 | đếm trùng lặp | 
| không khớp tiền tố | 0 | dãy trống | 
| nhiều chuỗi giống hệt nhau | tích lũy đúng | xử lý tần số | 
| tiền tố hỗn hợp | phân đoạn chính xác | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các từ trong từ điển đều giống hệt nhau. Đối với đầu vào trong đó tất cả các từ là "a" và truy vấn là "a", cả hai ranh giới đều thu gọn thành phạm vi mảng đầy đủ, do đó kết quả sẽ bằng số lần xuất hiện một cách chính xác. 

Một trường hợp đặc biệt khác là khi truy vấn có từ điển lớn hơn bất kỳ từ nào trong từ điển. Ví dụ: từ điển ["a", "b"] và truy vấn "z". Cả hai ranh giới tìm kiếm nhị phân đều trả về phần cuối của mảng, tạo ra số 0 mà không cần xử lý đặc biệt. 

Trường hợp cuối cùng là khi bản thân truy vấn là một từ đầy đủ có trong từ điển. Bởi vì ranh giới bên trái bao gồm đẳng thức, từ phù hợp được tính chính xác và ranh giới bên phải chỉ loại trừ các phần mở rộng không khớp, đảm bảo kết quả khớp chính xác được đưa vào kết quả.
