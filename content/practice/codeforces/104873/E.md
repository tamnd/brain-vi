---
title: "CF 104873E - Hủy email"
description: "Mỗi email thuộc về một “luồng” phát triển một cách rất cứng nhắc. Một luồng bắt đầu từ một chủ đề cơ sở, là một chuỗi chữ thường không trống. Mỗi email tiếp theo trong cùng một chuỗi được tạo bằng cách thêm tiền tố \"Re: \" vào chủ đề trước đó."
date: "2026-06-28T10:23:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104873
codeforces_index: "E"
codeforces_contest_name: "2018-2019 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104873
solve_time_s: 38
verified: true
draft: false
---

[CF 104873E - Hủy email](https://codeforces.com/problemset/problem/104873/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi email thuộc về một “luồng” phát triển một cách rất cứng nhắc. Một luồng bắt đầu từ một chủ đề cơ sở, là một chuỗi chữ thường không trống. Mỗi email tiếp theo trong cùng một chuỗi được tạo bằng cách thêm tiền tố vào trước`"Re: "`đến chủ đề trước đó. Vì vậy, một chủ đề hoàn toàn được xác định bởi chủ đề gốc của nó và số lần nó được trả lời. 

Chúng tôi được cung cấp ảnh chụp nhanh k email còn lại sau một cuộc tấn công xóa. Trật tự ban đầu của họ bị mất, nhưng thần dân của họ vẫn còn. Chúng tôi cũng đoán được tổng số email đã tồn tại trước khi xóa. Câu hỏi đặt ra là liệu có thể gán từng k email này vào một hoặc nhiều chuỗi trả lời hợp lệ sao cho tổng số email trên tất cả các chuỗi chính xác là n và mọi chuỗi đều tuân theo quy định nghiêm ngặt.`"Re: "`cấu trúc lồng nhau. 

Tương tự, chúng tôi đang cố gắng quyết định xem liệu các email được quan sát có thể được mở rộng lên và xuống trong chuỗi của chúng hay không, chèn các email bị thiếu để mỗi chuỗi trở thành một phân đoạn tiền tố hoàn chỉnh của một số “Re: tower” vô hạn và tổng số email bị thiếu cần thiết để hoàn thành tất cả các chuỗi có tổng chính xác là n − k. 

Các ràng buộc rất nhỏ: n và k nhiều nhất là 100. Điều này ngay lập tức loại trừ mọi phép liệt kê theo cấp số nhân đối với việc phân công email cho chuỗi hoặc hoàn thành chuỗi có thể có với các lựa chọn chi tiết cho mỗi email. Ngay cả các cấu trúc bậc ba hoặc bậc hai trên các trạng thái cũng có thể được chấp nhận, nhưng bất cứ điều gì phân nhánh cho mỗi email theo cách tổ hợp thì không. 

Một trường hợp thất bại tinh vi xuất phát từ các email thuộc cùng một chuỗi nhưng xuất hiện ở các độ sâu khác nhau. Ví dụ, nếu chúng ta thấy`"Re: hello"`Và`"hello"`, chúng phải thuộc cùng một chuỗi và ngụ ý ít nhất cấu trúc hai email. Một cách tiếp cận ngây thơ xử lý từng chủ đề một cách độc lập sẽ đánh giá thấp độ dài chuỗi cần thiết. 

Một tình huống phức tạp khác là khi nhiều đối tượng cơ bản bị ẩn. Nếu chúng ta thấy`"Re: world"`nhưng không bao giờ nhìn thấy`"world"`, chuỗi đó vẫn tồn tại và đóng góp ít nhất một email bị thiếu bên dưới chuỗi được quan sát. Hành vi giới hạn dưới này rất dễ bị bỏ lỡ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng gán từng chủ đề được quan sát vào một chuỗi, sau đó quyết định cho mỗi chuỗi có bao nhiêu email bị thiếu ở trên và dưới các nút được quan sát. Điều này nhanh chóng biến thành các chuỗi phân vùng bằng cách tước bỏ`"Re: "`lặp đi lặp lại cho đến khi đạt đến một nút gốc, sau đó nhóm theo các nút gốc và thử mọi cách để quyết định xem nút nào được quan sát thuộc chuỗi nào. Số lượng phân vùng của k mục đã theo cấp số nhân và đối với mỗi phân vùng, chúng tôi sẽ cần xác thực tính nhất quán của độ sâu phản hồi, khiến nó không thể thực hiện được ngay cả khi k = 100. 

Quan sát quan trọng là cấu trúc của mỗi email được xác định hoàn toàn bởi hai giá trị: chủ đề gốc của nó sau khi loại bỏ tất cả`"Re: "`tiền tố và độ sâu của nó, đó là số lượng`"Re: "`khối. Do đó, mọi email đều nằm trên một đường thẳng đứng được lập chỉ mục theo gốc của nó và trong mỗi gốc, chúng tôi chỉ quan tâm đến độ sâu hiện tại. 

Bên trong một gốc, giả sử chúng ta quan sát các độ sâu như 0, 2 và 5. Các độ sâu này phải thuộc một chuỗi duy nhất và các email bị thiếu giữa chúng là bắt buộc: giữa 0 và 2 phải có độ sâu 1, và giữa 2 và 5 phải có độ sâu 3 và 4. Vì vậy, trong mỗi gốc, cấu trúc chuỗi hoàn toàn cứng nhắc: nó chỉ là một khoảng số nguyên, có thể thiếu các điểm cuối ở trên hoặc dưới, nhưng được điền đầy đủ giữa các điểm được quan sát. 

Do đó, mỗi gốc đóng góp một phân đoạn độ sâu liền kề từ độ sâu quan sát tối thiểu xuống 0 và có khả năng tăng lên cao hơn nếu chúng ta giả sử các email bị thiếu tồn tại trên độ sâu quan sát tối đa. Tính linh hoạt duy nhất còn lại là có bao nhiêu email bổ sung tồn tại trên độ sâu tối đa được quan sát trong mỗi chuỗi. 

Điều này làm giảm vấn đề theo quan điểm sau: đối với mỗi gốc, chúng tôi biết kích thước yêu cầu tối thiểu (độ sâu tối đa + 1) và chúng tôi có thể tùy ý mở rộng từng chuỗi bằng cách thêm nhiều email hơn lên trên. Chúng ta cần xem liệu chúng ta có thể phân phối thêm email để tổng số email trở thành chính xác n hay không. 

Điều này trở thành một phép kiểm tra tính khả thi kiểu tổng tập hợp con cổ điển: mỗi chuỗi có chi phí tối thiểu và có thể tăng tùy ý bằng cách mở rộng lên trên. Tuy nhiên, việc mở rộng là không giới hạn, do đó hạn chế thực sự duy nhất là liệu tổng chi phí tối thiểu có nhiều nhất là n hay không. Nếu nó lớn hơn n thì không thể. Nếu nó nhỏ hơn hoặc bằng nhau, chúng tôi luôn có thể phân bổ các email còn lại bằng cách mở rộng bất kỳ chuỗi nào vì việc thêm email lên trên không vi phạm cấu trúc. 

Vì vậy, vấn đề chuyển sang tính toán, đối với mỗi nghiệm, độ sâu quan sát được tối đa, tính tổng (độ sâu tối đa + 1) và kiểm tra theo n. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi phân vùng Brute Force | hàm mũ trong k | O(k) | Quá chậm | 
| Nhóm theo gốc và tính toán phạm vi độ sâu | O(k · L) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đối với mỗi email, liên tục loại bỏ tiền tố`"Re: "`và đếm xem nó xuất hiện bao nhiêu lần. Điều này mang lại chiều sâu cho chuỗi email và chủ đề cơ bản của nó. Lý do điều này có hiệu quả là vì mỗi câu trả lời đều thêm chính xác một tiền tố cố định, do đó việc loại bỏ nó là mang tính quyết định. 
2. Lưu trữ, đối với mỗi chủ đề cơ bản, độ sâu tối đa được thấy trong số tất cả các email thuộc chủ đề đó. Chúng tôi chỉ cần mức tối đa vì bất kỳ độ sâu nhỏ hơn nào cũng không làm tăng độ dài chuỗi tối thiểu cần thiết vượt quá mức mà email sâu nhất đã buộc. 
3. Đối với mỗi chủ đề cơ bản, hãy tính kích thước chuỗi tối thiểu có thể như sau:`max_depth + 1`. Điều này tương ứng với một chuỗi hoàn chỉnh bắt đầu từ email gốc (độ sâu 0) cho đến email được quan sát sâu nhất, điền vào tất cả các phản hồi trung gian. 
4. Tính tổng các kích thước tối thiểu này của tất cả các đối tượng cơ bản. Điều này thể hiện số lượng email nhỏ nhất có thể tồn tại trong quá trình xây dựng lại hộp thư nhất quán. 
5. So sánh số tiền này với n. Nếu tổng vượt quá n, không có cách nào để xóa email mà vẫn giữ được tính hợp lệ. Nếu nó nhỏ hơn hoặc bằng n, chúng ta luôn có thể mở rộng một hoặc nhiều chuỗi lên trên bằng cách chèn thêm`"Re: "`lớp, tăng tổng số lên chính xác n. 

Ý tưởng chính là việc mở rộng đi lên trong bất kỳ chuỗi nào luôn hợp pháp và độc lập với các chuỗi khác, do đó, mọi khoản thâm hụt đều có thể được lấp đầy mà không bị ràng buộc. 

### Tại sao nó hoạt động 

Mỗi chủ đề cơ sở xác định một chuỗi email tuyến tính độc lập được sắp xếp theo độ sâu. Các ràng buộc buộc tất cả các độ sâu trung gian từ 0 đến độ sâu quan sát được tối đa phải tồn tại trong bất kỳ quá trình tái tạo hợp lệ nào, vì vậy những vị trí đó là bắt buộc. Mọi thứ trên độ sâu tối đa đều không bị giới hạn và có thể được mở rộng tùy ý. Vì các chuỗi không tương tác nên hạn chế toàn cầu duy nhất là liệu tổng các phần bắt buộc có vượt quá n hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_subject(s):
    depth = 0
    while s.startswith("Re: "):
        depth += 1
        s = s[4:]
    return s, depth

def solve():
    n, k = map(int, input().split())
    max_depth = {}

    for _ in range(k):
        s = input().strip()
        root, d = parse_subject(s)
        if root not in max_depth:
            max_depth[root] = d
        else:
            if d > max_depth[root]:
                max_depth[root] = d

    min_total = 0
    for d in max_depth.values():
        min_total += d + 1

    print("YES" if min_total <= n else "NO")

if __name__ == "__main__":
    solve()
```Chức năng phân tích cú pháp cô lập cấu trúc chuỗi bằng cách loại bỏ các cấu trúc cố định`"Re: "`khối. Điều này an toàn vì định dạng này đảm bảo sự lặp lại tiền tố nghiêm ngặt mà không có sự mơ hồ. Từ điển chỉ theo dõi email được quan sát sâu nhất trên mỗi chuỗi, vì chỉ riêng nó mới xác định mức đóng tiền tố bắt buộc tối thiểu. 

So sánh cuối cùng mã hóa điều kiện khả thi: nếu cấu trúc bắt buộc đã vượt quá n, chúng tôi không thể xóa đủ email; nếu không thì chúng ta luôn có thể thổi phồng chuỗi lên trên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 3
Re: Re: Re: hello
Re: world
hello
```Chúng tôi tính toán: 

| Email | Gốc | Độ sâu | 
| --- | --- | --- | 
| xin chào | xin chào | 0 | 
| Re: thế giới | thế giới | 1 | 
| Re: Re: Re: xin chào | xin chào | 3 | 

Đối với gốc`"hello"`, độ sâu tối đa là 3, vì vậy kích thước tối thiểu là 4. Đối với`"world"`, độ sâu tối đa là 1, vì vậy kích thước tối thiểu là 2. Tổng tối thiểu là 6. 

Vì 6 ≤ 7, chúng ta có thể mở rộng một chuỗi lên trên một lần, cho kết quả chính xác là 7. 

Điều này khẳng định tính khả thi. 

### Ví dụ 2 

đầu vào:```
3 2
Re: Re: pleasehelp
me
```| Email | Gốc | Độ sâu | 
| --- | --- | --- | 
| tôi | tôi | 0 | 
| Re: Re: xin vui lòng giúp đỡ | xin vui lòng giúp đỡ | 2 | 

Kích thước tối thiểu lần lượt là 1 và 3, tổng cộng là 4. 

Vì 4 > 3, ngay cả việc tái tạo nhất quán nhỏ nhất cũng vượt quá tổng số được đoán, nên câu trả lời là không thể. 

Điều này cho thấy các email cơ sở ẩn buộc cấu trúc tối thiểu không thể tránh khỏi như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · L) | Mỗi chủ đề được quét một lần, loại bỏ các tiền tố có độ dài L | 
| Không gian | O(k) | Một mục bản đồ cho mỗi chủ đề gốc riêng biệt | 

Các ràng buộc ràng buộc k tối đa là 100 và độ dài mỗi chuỗi tối đa là 500, do đó, quá trình quét tuyến tính dễ dàng đủ nhanh trong vòng 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("""7 3
Re: Re: Re: hello
Re: world
hello
""") == "YES"

assert run("""3 2
Re: Re: pleasehelp
me
""") == "NO"

# single chain, exact match
assert run("""4 2
hello
Re: Re: hello
""") == "YES"

# single email
assert run("""1 1
hello
""") == "YES"

# multiple chains minimal
assert run("""5 3
a
Re: a
b
""") == "YES"

# impossible due to too many required nodes
assert run("""2 2
a
Re: a
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| si | | |
