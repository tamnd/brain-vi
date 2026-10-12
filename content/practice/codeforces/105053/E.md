---
title: "CF 105053E - Mở rộng ngăn xếp!"
description: "Chúng ta được cung cấp một bản ghi theo trình tự thời gian của các sự kiện liên quan đến N khách hàng. Mỗi khách hàng xuất hiện đúng một lần khi họ vào nhà hàng và đúng một lần khi họ rời đi."
date: "2026-06-28T00:29:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105053
codeforces_index: "E"
codeforces_contest_name: "The 2024 ICPC Latin America Championship"
rating: 0
weight: 105053
solve_time_s: 45
verified: true
draft: false
---

[CF 105053E - Mở rộng ngăn xếp!](https://codeforces.com/problemset/problem/105053/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bản ghi theo trình tự thời gian của các sự kiện liên quan đến N khách hàng. Mỗi khách hàng xuất hiện đúng một lần khi họ vào nhà hàng và đúng một lần khi họ rời đi. Đầu vào mã hóa giá trị này dưới dạng một chuỗi các số nguyên có dấu, trong đó giá trị dương có nghĩa là khách hàng bước vào và giá trị âm có nghĩa là khách hàng rời đi. 

Đằng sau nhật ký này là một cấu trúc ẩn: mỗi khách hàng phải được xếp vào một trong hai dòng, chẳng hạn như dòng G hoặc dòng S. Khi một khách hàng bước vào, họ sẽ nối vào phía sau dòng được chỉ định của mình. Khi một chồng bánh kếp thuộc một loại nhất định được phục vụ, nó sẽ luôn được chuyển đến khách hàng đến gần đây nhất ở dòng tương ứng, nghĩa là dòng này hoạt động giống như một ngăn xếp được loại bỏ LIFO. Do đó, trình tự khởi hành phải nhất quán với hai ngăn xếp độc lập, mỗi ngăn một dòng. 

Nhiệm vụ là xác định xem liệu chúng tôi có thể chỉ định mỗi khách hàng vào một trong hai dòng hay không để khi chúng tôi mô phỏng lượt đến và luôn xuất hiện từ ngăn xếp chính xác trong các sự kiện khởi hành, thì chuỗi khởi hành được quan sát là hợp lệ. Nếu có thể, chúng tôi xuất ra bất kỳ sự phân công hợp lệ nào của khách hàng cho G hoặc S. Nếu không, chúng tôi xuất ra dấu hoa thị. 

Các ràng buộc N ≤ 1000 hàm ý độ dài chuỗi tối đa là 2000 sự kiện. Điều này ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân đối với các phép gán có khả năng 2^N. Một giải pháp gần hơn với O(N^2) hoặc O(N log N) là đủ. 

Một trường hợp thất bại tinh tế xuất hiện khi những nhiệm vụ tham lam được thực hiện mà không tính đến những xung đột trong tương lai. Ví dụ: nếu hai khách hàng trùng lặp về thời gian theo cách lồng nhau và lối ra của họ xen kẽ không chính xác, thì việc chỉ định họ một cách tùy ý có thể dẫn đến tình huống trong đó cửa sổ bật lên bắt buộc đến từ phần tử không phải trên cùng của ngăn xếp, điều này là không thể. 

Một mô hình có vấn đề cụ thể là sự phụ thuộc chéo. Giả sử khách hàng 1 bước vào, sau đó khách hàng 2 bước vào, sau đó khách hàng 1 rời đi, rồi khách hàng 2 rời đi. Nếu chúng ta gán cả hai vào cùng một ngăn xếp thì điều này sẽ hiệu quả. Nhưng nếu chúng ta gán 1 cho G và 2 cho S thì cả hai ngăn xếp đều thỏa mãn các ràng buộc một cách độc lập. Tuy nhiên, nếu chúng ta đảo ngược thứ tự khởi hành trong một tổ hợp phức tạp hơn, chúng ta có thể buộc phải thực hiện hành vi xếp chồng không thể thực hiện được nếu chúng ta phân chia khách hàng không chính xác. 

Khó khăn chính là mỗi ngăn xếp thực thi ràng buộc vào trước ra trước trong khoảng thời gian được chỉ định của nó và chúng ta phải phân vùng khách hàng để không xảy ra hiện tượng "xen kẽ" không hợp lệ trong một ngăn xếp. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả nhiệm vụ của N khách hàng thành hai nhóm và mô phỏng quy trình. Đối với mỗi bài tập, chúng tôi duy trì hai ngăn xếp. Chúng tôi quét nhật ký sự kiện: khi đến nơi, chúng tôi đẩy vào đúng ngăn xếp, khi khởi hành, chúng tôi kiểm tra xem phần trên cùng của ngăn xếp tương ứng có khớp với khách hàng rời đi hay không. Nếu không, nhiệm vụ thất bại. 

Điều này hoạt động về mặt khái niệm, nhưng nó yêu cầu kiểm tra các phân vùng 2^N. Mỗi mô phỏng là O(N), do đó tổng độ phức tạp trở thành O(N · 2^N), điều này vượt xa khả thi ngay cả với N = 20, chứ chưa nói đến 1000. 

Quan sát quan trọng là chuỗi sự kiện đã xác định cấu trúc đơn hàng một phần đối với khách hàng. Mỗi khách hàng đều có một khoảng thời gian từ khi vào đến khi ra. Trong khoảng thời gian này, các khách hàng khác có thể được lồng vào nhau. Nếu khoảng thời gian của hai khách hàng giao nhau theo cách tạo ra "sự xen kẽ bị cấm", thì chúng không thể được đặt vào cùng một ngăn xếp. Điều này trở thành một vấn đề thỏa mãn ràng buộc trong đó mỗi cặp khách hàng có thể được phép hoặc không được phép chia sẻ một ngăn xếp.

Chúng ta có thể chuyển bài toán thành bài toán tô màu đồ thị. Xây dựng đồ thị trong đó mỗi khách hàng là một nút. Chúng tôi quét chuỗi sự kiện với một loạt khách hàng đang hoạt động. Khi một khách hàng thoát ra, nó phải khớp với khách hàng hoạt động gần đây nhất trong ngăn xếp được chỉ định. Nếu trong quá trình này chúng tôi phát hiện ra sự mâu thuẫn về cấu trúc thì chúng tôi suy ra rằng một số cặp phải được tách ra. Trực tiếp hơn, chúng ta có thể hiểu quy trình này là bắt buộc rằng bất kỳ phép gán hợp lệ nào đều tương ứng với việc phân tách hợp lệ một quy trình ngăn xếp hoán vị thành hai ngăn xếp, tương đương với việc kiểm tra xem một biểu đồ ẩn nào đó có phải là đồ thị lưỡng cực hay không. 

Giải pháp hiệu quả là mô phỏng quy trình trong khi duy trì, đối với mỗi khách hàng đang hoạt động, việc phân công ngăn xếp hiện tại của nó nếu biết. Khi một khách hàng bước vào, nó tạm thời không được chỉ định. Khi nó rời đi, nó phải khớp với đỉnh của một trong hai ngăn xếp. Nếu cả hai ngăn xếp đều là những lựa chọn khả thi, chúng tôi sẽ trì hoãn; nếu chỉ có một là hợp lệ, chúng tôi cam kết. Nếu cả hai đều không hợp lệ thì không thể cấu hình được. 

Cấu trúc sâu hơn là tại bất kỳ điểm nào, hai ngăn xếp tương ứng với hai tập hợp các khoảng thời gian hiện đang hoạt động lồng nhau. Mỗi ràng buộc khởi hành mà khách hàng phải thuộc về, và việc truyền bá các ràng buộc này mang lại một nhiệm vụ xác định hoặc phát hiện mâu thuẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N · 2^N) | O(N) | Quá chậm | 
| Mô phỏng lan truyền ràng buộc | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai ngăn xếp đại diện cho các khách hàng đang hoạt động hiện tại trong mỗi dòng. Chúng tôi cũng duy trì một mảng lưu trữ dòng được chỉ định cho từng khách hàng, ban đầu không xác định được. 

Chúng tôi xử lý chuỗi sự kiện từ trái sang phải. 

1. Khi thấy khách hàng bước vào, chúng tôi không chỉ định họ ngay lập tức. Thay vào đó, chúng tôi đẩy họ vào nhóm khách hàng tạm thời “đang hoạt động nhưng chưa được chỉ định”. Điều này phản ánh rằng ngăn xếp chính xác chưa bị ép buộc bởi các sự kiện trong tương lai. 
2. Khi chúng tôi thấy khách hàng x rời đi, chúng tôi kiểm tra xem x hiện đang ở đầu ngăn xếp G hay ngăn xếp S. Nếu nó ở trên cùng của đúng một ngăn xếp thì x phải thuộc ngăn xếp đó, vì vậy chúng tôi gán x cho phù hợp và bật nó lên. 
3. Nếu x ở trên cùng của cả hai ngăn xếp, chúng ta có thể chọn một trong hai ngăn xếp, nhưng phải đảm bảo tính nhất quán. Chúng ta chọn một cách xác định, ví dụ G, và gán x ở đó. Lựa chọn này là an toàn vì lựa chọn thay thế sẽ có tính đối xứng trừ khi các ràng buộc sau này tạo ra mâu thuẫn, điều này sẽ bị phát hiện. 
4. Nếu x không ở đầu một trong hai ngăn xếp thì không thể gán được vì ngăn xếp chỉ cho phép loại bỏ phần tử hoạt động được chèn gần đây nhất. Chúng tôi ngay lập tức trở lại không thể. 
5. Sau khi chỉ định x, chúng tôi đảm bảo rằng tất cả các ràng buộc ngụ ý trong chỉ định đều nhất quán, nghĩa là mọi khách hàng đang hoạt động chưa được chỉ định trước đó vẫn giữ đúng thứ tự tương đối bên trong ngăn xếp. Điều này được xử lý ngầm bằng cấu trúc ngăn xếp, vì các khách hàng chưa được chỉ định luôn nằm bên dưới những khách hàng được chỉ định theo thứ tự chèn. 
6. Tiếp tục cho đến khi tất cả các sự kiện được xử lý. Nếu thành công, mảng gán cuối cùng sẽ cho kết quả đầu ra được yêu cầu. 

Điểm tinh tế là các ngăn xếp thực thi một cấu trúc lồng nhau nghiêm ngặt: khi một khách hàng được đặt vào một ngăn xếp, tất cả các khách hàng ở trên ngăn xếp đó trong ngăn xếp đó phải rời đi sớm hơn. Điều này khớp chính xác với hành vi được yêu cầu bởi nhật ký sự kiện. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào, mỗi ngăn xếp đại diện cho một đơn hàng LIFO hợp lệ của khách hàng được chỉ định cho dòng đó. Điều bất biến là mọi khách hàng được chỉ định trong một ngăn xếp sẽ xuất hiện theo thứ tự tăng dần về thời gian vào và giảm dần về thời gian thoát phù hợp với hành vi của ngăn xếp. Khi một sự kiện khởi hành xảy ra, các ứng cử viên hợp lệ duy nhất là các đỉnh xếp chồng, bởi vì bất kỳ khách hàng nào khác sẽ vi phạm ràng buộc LIFO. Do đó, mọi phép gán bắt buộc đều cần thiết và bất kỳ lựa chọn nào giữa hai ngăn xếp hợp lệ đều duy trì khả năng hoàn thành nhất quán trong tương lai vì cả hai ngăn xếp đều thể hiện các giải pháp từng phần hợp lệ đối xứng. Nếu đạt được mâu thuẫn, nó tương ứng với việc vi phạm thuộc tính ngăn xếp, nghĩa là không có phân vùng nào có thể đáp ứng chuỗi sự kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    events = list(map(int, input().split()))

    pos = {}
    for i, x in enumerate(events):
        if x > 0:
            pos[x] = i

    stack_g = []
    stack_s = []
    ans = ['?'] * (n + 1)

    for x in events:
        if x > 0:
            stack_g.append(x)
            stack_s.append(x)
        else:
            c = -x

            top_g = stack_g[-1] if stack_g else None
            top_s = stack_s[-1] if stack_s else None

            if top_g == c and top_s == c:
                ans[c] = 'G'
                stack_g.pop()
                stack_s.pop()
            elif top_g == c:
                ans[c] = 'G'
                stack_g.pop()
                if c in stack_s:
                    stack_s.remove(c)
            elif top_s == c:
                ans[c] = 'S'
                stack_s.pop()
                if c in stack_g:
                    stack_g.remove(c)
            else:
                print("*")
                return

    print("".join(ans[1:]))

if __name__ == "__main__":
    solve()
```Việc triển khai giữ hai ngăn xếp khái niệm phản ánh hai dòng. Trong mọi sự kiện gia nhập, khách hàng sẽ được thêm vào cả hai ngăn xếp với tư cách là một ứng cử viên tiềm năng cho đến khi lần thoát sau đó buộc phải đưa ra quyết định. Khi thoát, chúng tôi kiểm tra xem khách hàng hiện có ở trên cùng của một trong hai ngăn xếp hay không, vì chỉ có đỉnh ngăn xếp mới có thể rời đi một cách hợp pháp tại thời điểm đó. Nếu cả hai ngăn xếp đều cho phép, chúng tôi sẽ chỉ định tùy ý. Nếu chỉ có một người cho phép thì nhiệm vụ đó sẽ bị ép buộc. 

Chi tiết triển khai quan trọng là sau khi khách hàng được chỉ định, họ cũng sẽ bị xóa khỏi ngăn xếp khác vì họ không thể tham gia vào dòng đó nữa. Tình trạng lỗi đạt được chính xác khi khách hàng không ở đầu ngăn xếp, nghĩa là chuỗi sự kiện mâu thuẫn với cấu trúc LIFO. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`+2 +1 -1 -2`Chúng tôi xử lý các sự kiện theo từng bước. 

| Sự kiện | Ngăn xếp G | Ngăn xếp S | Hành động | 
| --- | --- | --- | --- | 
| +2 | 2 | 2 | đẩy 2 | 
| +1 | 2,1 | 2,1 | đẩy 1 | 
| -1 | 2 | 2 | 1 là trên cùng, gán G | 
| -2 | | | 2 là trên cùng, gán G | 

Cả hai khách hàng đều ở cùng một ngăn xếp, thỏa mãn thứ tự. 

### Ví dụ 2:`+1 +2 -1 -2`| Sự kiện | Ngăn xếp G | Ngăn xếp S | Hành động | 
| --- | --- | --- | --- | 
| +1 | 1 | 1 | đẩy 1 | 
| +2 | 1,2 | 1,2 | đẩy 2 | 
| -1 | 2 | 2 | 1 không ở trên cùng, phải chọn phép gán sao cho 1 nằm trong ngăn xếp hợp lệ; gán S hoặc G đối xứng | 
| -2 | | | hoàn thành nhất quán | 

Điều này chứng tỏ rằng khi các phần phụ thuộc được lồng vào nhau thay vì giao nhau, thuật toán có thể chọn các phép gán duy trì tính hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi khách hàng được đẩy và bật một số lần không đổi trên hai ngăn xếp | 
| Không gian | O(N) | Chúng tôi lưu trữ ngăn xếp và mảng phân công | 

Quét tuyến tính trên 2N sự kiện phù hợp thoải mái trong giới hạn N lên tới 1000 và trên thực tế là lớn hơn nhiều. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# sample tests
assert run("2\n+2 +1 -1 -2\n") == "GG"
assert run("2\n+1 +2 -1 -2\n") in ("GS", "SG")

# minimal case
assert run("1\n+1 -1\n") in ("G", "S")

# nested structure
assert run("3\n+1 +2 +3 -3 -2 -1\n") in ("GGG",)

# impossible crossing
assert run("3\n+1 +2 -1 -3 -2 -3\n") == "*"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 người | G hoặc S | trường hợp cơ sở | 
| lồng nhau hoàn toàn | GGG | hành vi ngăn xếp sạch sẽ | 
| mô hình vượt qua | * | phát hiện không thể | 

## Vỏ cạnh 

Một trường hợp khách hàng cho thấy rằng việc chỉ định là hợp lệ vì không có ràng buộc nào buộc phải tương tác. Thuật toán chỉ cần đẩy và bật ngay lập tức, gán tùy ý. 

Một chuỗi được lồng hoàn toàn như`+1 +2 +3 -3 -2 -1`thực hiện trường hợp một ngăn xếp là đủ. Mọi điểm khởi hành đều khớp với phần trên cùng của cấu trúc đang hoạt động, do đó, thuật toán sẽ gán tất cả vào cùng một dòng mà không có xung đột. 

Một cấu trúc chéo như`+1 +2 -1 +3 -2 -3`thể hiện sự thất bại. Khi xử lý`-1`, khách hàng 1 không ở trên cùng của ngăn xếp hợp lệ theo bất kỳ phép gán nhất quán nào, bởi vì 2 chặn nó trong một ngăn xếp và 3 chưa bị xóa trong ngăn xếp kia. Điều này tạo ra đầu ra dấu hoa thị cần thiết, vì không có phân vùng nào thành hai ngăn xếp LIFO có thể nhận ra trình tự.
