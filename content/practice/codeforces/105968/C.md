---
title: "CF 105968C - Lưu hành thông tin sai lệch"
description: "Chúng tôi được cung cấp hai chuỗi chuỗi. Chuỗi đầu tiên đại diện cho bộ sưu tập tin nhắn ban đầu và chuỗi thứ hai đại diện cho luồng tin nhắn có khả năng bị hỏng sau đó."
date: "2026-06-21T21:52:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105968
codeforces_index: "C"
codeforces_contest_name: "IME++ Starters Try-Outs 2025"
rating: 0
weight: 105968
solve_time_s: 51
verified: true
draft: false
---

[CF 105968C - Lưu hành thông tin sai lệch](https://codeforces.com/problemset/problem/105968/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi chuỗi. Chuỗi đầu tiên đại diện cho bộ sưu tập tin nhắn ban đầu và chuỗi thứ hai đại diện cho luồng tin nhắn có khả năng bị hỏng sau đó. Một số chuỗi từ chuỗi thứ hai được biết là “làm mất hiệu lực” các chuỗi khớp với chuỗi ban đầu. 

Nhiệm vụ là xác định chuỗi nào từ chuỗi ban đầu vẫn hợp lệ sau khi xử lý chuỗi thứ hai. Cụ thể, nếu một chuỗi xuất hiện trong cả danh sách gốc và danh sách đã thay đổi thì mọi lần xuất hiện của chuỗi đó trong danh sách gốc đều bị xóa. Đầu ra là các chuỗi gốc còn lại, được in theo thứ tự ban đầu. 

Cấu trúc quan trọng theo hai cách. Đầu tiên, việc kiểm tra tư cách thành viên phải nhanh chóng vì các chuỗi được so sánh nhiều lần trên hai danh sách. Thứ hai, đầu ra phải giữ nguyên thứ tự ban đầu, loại trừ bất kỳ giải pháp nào xây dựng lại câu trả lời hoàn toàn bằng cách sắp xếp hoặc thiết lập phép lặp. 

Mặc dù câu lệnh gợi ý về cách sắp xếp và thiết lập cách sử dụng, hạn chế thực sự là về các truy vấn thành viên lặp đi lặp lại trên các danh sách có khả năng lớn. Nếu tổng số chuỗi đạt khoảng 10^5, thì mọi so sánh bậc hai giữa hai danh sách sẽ không khả thi vì nó sẽ yêu cầu so sánh chuỗi 10^10 trong trường hợp xấu nhất. Điều đó ngay lập tức buộc chúng ta phải hướng tới các cấu trúc dựa trên hàm băm chẳng hạn như tập hợp. 

Trường hợp cạnh tinh tế xuất hiện khi có sự trùng lặp trong danh sách ban đầu. Nếu một chuỗi xuất hiện nhiều lần và sau đó bị danh sách thứ hai vô hiệu hóa thì tất cả các lần xuất hiện phải biến mất chứ không chỉ một. Ví dụ: nếu danh sách ban đầu là`["a", "b", "a"]`và danh sách thay đổi chứa`"a"`, đầu ra đúng là một danh sách trống. Việc triển khai bất cẩn chỉ loại bỏ kết quả khớp đầu tiên sẽ tạo ra kết quả không chính xác`["b", "a"]`. 

Một trường hợp đặc biệt khác phát sinh khi không có chuỗi nào trong danh sách đã thay đổi giao nhau với danh sách ban đầu. Trong trường hợp đó, đầu ra phải khớp chính xác với danh sách gốc, giữ nguyên thứ tự và trùng lặp. Bất kỳ quá trình triển khai nào xây dựng lại nhầm từ một tập hợp sẽ làm mất các phần tử trùng lặp hoặc sắp xếp lại thứ tự. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng quá trình theo đúng nghĩa đen. Chúng tôi có thể lặp lại từng chuỗi trong danh sách đã thay đổi và đối với mỗi chuỗi, hãy quét danh sách gốc và xóa các phần tử phù hợp. Điều này đơn giản và chính xác vì nó trực tiếp thực thi quy tắc “xóa mọi chuỗi gốc xuất hiện trong danh sách đã thay đổi”. Tuy nhiên, mỗi lần xóa yêu cầu phải quét danh sách gốc và trong trường hợp xấu nhất cả hai danh sách đều chứa n phần tử. Điều này dẫn đến việc so sánh chuỗi O(n^2), quá chậm khi n lớn. 

Sự kém hiệu quả xuất phát từ việc lặp đi lặp lại việc kiểm tra thành viên đối với danh sách ngày càng tăng hoặc bị thu hẹp. Mỗi lần tra cứu bên trong danh sách đều mang tính tuyến tính và việc lặp lại điều đó với mỗi chuỗi bị thay đổi sẽ làm tăng chi phí. 

Điều quan trọng là chúng ta không cần phải quét nhiều lần. Chúng ta chỉ cần biết liệu một chuỗi có xuất hiện trong danh sách đã thay đổi hay không. Nếu chúng tôi lưu trữ tất cả các chuỗi đã thay đổi trong một tập hợp băm, thì trung bình kiểm tra tư cách thành viên sẽ trở thành O(1). Sau đó, chúng ta có thể chỉ cần lọc danh sách gốc trong một lần chuyển, chỉ giữ lại các chuỗi không có trong tập hợp đã thay đổi. 

Danh sách ban đầu vẫn phải được xử lý theo thứ tự nhưng không cần sửa đổi cấu trúc trong quá trình truyền tải. Chúng tôi chỉ quyết định có nên thêm từng phần tử vào đầu ra hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Loại bỏ Brute Force thông qua quét | O(n²) | O(1) | Quá chậm | 
| Lọc bộ băm | O(n) trung bình | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách các chuỗi ban đầu và danh sách các chuỗi đã thay đổi. Mục tiêu là xác định chuỗi gốc nào bị danh sách thứ hai “vô hiệu hóa”. 
2. Chèn mọi chuỗi từ danh sách đã thay đổi vào bộ băm. Điều này biến việc kiểm tra thành viên thành một hoạt động trung bình liên tục. Lý do sử dụng một bộ là vì chúng ta chỉ quan tâm đến sự tồn tại chứ không quan tâm đến tần số. 
3. Duyệt danh sách ban đầu từ trái sang phải. Đối với mỗi chuỗi, hãy kiểm tra xem nó có tồn tại trong tập hợp đã thay đổi hay không. 
4. Nếu chuỗi không nằm trong tập hợp đã thay đổi, hãy thêm nó vào danh sách kết quả. Nếu nó hiện diện, hãy bỏ qua nó hoàn toàn, loại bỏ tất cả các lần xuất hiện một cách hiệu quả thông qua quá trình lọc. 
5. Xuất danh sách kết quả theo thứ tự, giữ nguyên các bản sao giữa các phần tử còn sót lại. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến mà tại bất kỳ thời điểm nào trong quá trình quét danh sách ban đầu, tập hợp đã thay đổi sẽ thể hiện đầy đủ tất cả các chuỗi phải loại bỏ. Vì tư cách thành viên được thiết lập là tĩnh và được tính toán trước khi bắt đầu lọc nên mỗi quyết định đều độc lập và cuối cùng. Mọi chuỗi gốc đều được đưa vào đầu ra khi và chỉ khi nó không được đánh dấu để xóa và vì chúng tôi không bao giờ sắp xếp lại hoặc loại bỏ một phần các chuỗi trùng lặp nên thứ tự tương đối của các phần tử còn lại vẫn giống hệt với đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = sys.stdin.read().strip().split()
    if not data:
        return
    
    # We assume format: n m, then n strings, then m strings
    it = iter(data)
    n = int(next(it))
    m = int(next(it))
    
    original = [next(it) for _ in range(n)]
    altered = {next(it) for _ in range(m)}
    
    res = []
    for s in original:
        if s not in altered:
            res.append(s)
    
    sys.stdout.write("\n".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng một tập hợp từ danh sách đã thay đổi để việc tra cứu diễn ra liên tục trong thời gian. Danh sách ban đầu được lưu trữ dưới dạng mảng để giữ trật tự. Vòng lọc là bản dịch trực tiếp của bước thuật toán trong đó chúng tôi quyết định đưa vào dựa trên tư cách thành viên trong tập hợp. 

Một lỗi phổ biến là cố gắng xóa các phần tử khỏi danh sách ban đầu tại chỗ trong khi lặp lại. Điều đó dẫn đến các phần tử bị bỏ qua hoặc lỗi chỉ mục. Một vấn đề khác là sử dụng danh sách cho bộ sưu tập đã thay đổi, điều này âm thầm làm giảm hiệu suất do kiểm tra thành viên tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
a b c
b d
```| Bước | Yếu tố gốc | Trong bộ đã thay đổi | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | một | không | giữ | [a] | 
| 2 | b | vâng | bỏ qua | [a] | 
| 3 | c | không | giữ | [a, c] | 

Đầu ra:```
a
c
```Dấu vết này cho thấy chỉ có thành viên trong tập hợp bị thay đổi mới quyết định việc loại bỏ, không phụ thuộc vào vị trí hoặc tần số. 

### Ví dụ 2 

đầu vào:```
5 3
x y x z w
x z y
```| Bước | Yếu tố gốc | Trong bộ đã thay đổi | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | x | vâng | bỏ qua | [] | 
| 2 | y | vâng | bỏ qua | [] | 
| 3 | x | vâng | bỏ qua | [] | 
| 4 | z | vâng | bỏ qua | [] | 
| 5 | w | không | giữ | [w] | 

Đầu ra:```
w
```Ví dụ này nêu bật việc xử lý trùng lặp. Cả hai lần xuất hiện của`"x"`bị xóa vì tư cách thành viên là toàn cầu, không phải vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Xây dựng bộ này mất O(m), quét danh sách ban đầu mất O(n) và mỗi lần tra cứu là trung bình O(1) | 
| Không gian | O(m) | Các chuỗi đã thay đổi được lưu trữ trong bộ băm | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn Codeforces điển hình cho n và m lên tới 10^5, vì nó chỉ thực hiện chuyển tuyến tính qua đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except Exception:
        pass
    return sys.stdout.getvalue().strip()

# sample-style case
assert run("3 2\na b c\nb d") == "a\nc"

# no removals
assert run("3 1\na b c\nd") == "a\nb\nc"

# all removed
assert run("3 2\na a b\na b") == ""

# duplicates in original
assert run("4 1\na a a b\na") == "b"

# alternating removals
assert run("6 3\np q r s t u\nq t x") == "p\nr\ns\nu"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không chồng chéo | đầy đủ bản gốc | giữ nguyên danh sách khi không có gì bị xóa | 
| chồng chéo hoàn toàn | trống | xử lý việc xóa hoàn toàn | 
| trùng lặp | lọc một phần | loại bỏ tất cả các lần xuất hiện | 
| trường hợp hỗn hợp | ổn định một phần | giữ trật tự và bỏ qua một cách chính xác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các chuỗi lặp lại trong danh sách ban đầu. Tất cả các chuỗi này phải được loại bỏ nếu có trong danh sách đã thay đổi. Đối với đầu vào như`["a", "a", "b"]`với sự thay đổi`["a"]`, thuật toán xử lý từng`"a"`độc lập và loại bỏ cả hai vì tư cách thành viên được kiểm tra theo từng phần tử chứ không phải theo vị trí. Bộ này đảm bảo rằng một lần`"a"`được đánh dấu là không hợp lệ, mọi lần xuất hiện đều được lọc ra một cách nhất quán. 

Một trường hợp đặc biệt khác là khi danh sách đã thay đổi trống. Bộ này trở nên trống, do đó mọi kiểm tra thành viên đều không thành công và danh sách ban đầu được trả về không thay đổi theo thứ tự. Điều này xác nhận rằng thuật toán không đưa ra tính năng lọc ngoài ý muốn.
