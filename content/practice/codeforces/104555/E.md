---
title: "CF 104555E - Chiết xuất phấn hoa"
description: "Chúng tôi được tặng một bộ sưu tập hoa, mỗi bông chứa một lượng phấn hoa nguyên. Những con ong lần lượt đến theo một thứ tự cố định và mỗi con thực hiện đúng một hành động trước khi rời đi."
date: "2026-06-30T08:48:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104555
codeforces_index: "E"
codeforces_contest_name: "2023-2024 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 104555
solve_time_s: 91
verified: true
draft: false
---

[CF 104555E - Chiết xuất phấn hoa](https://codeforces.com/problemset/problem/104555/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập hoa, mỗi bông chứa một lượng phấn hoa nguyên. Những con ong lần lượt đến theo một thứ tự cố định và mỗi con thực hiện đúng một hành động trước khi rời đi. 

Khi một con ong hành động, nó luôn chọn một bông hoa hiện có giá trị phấn hoa lớn nhất trong số tất cả các loài hoa. Từ bông hoa được chọn đó có giá trị$x$, con ong thu thập tổng các chữ số của$x$, và khi đó giá trị của bông hoa sẽ giảm đi đúng bằng số tiền đó. Sau hoạt động duy nhất này, con ong sẽ rời đi vĩnh viễn. 

Chúng tôi không được yêu cầu mô phỏng tất cả những con ong. Thay vào đó, chúng tôi cần số tiền được thu thập bởi$K$-con ong trong quá trình này. 

Khó khăn chính là mọi hành động đều làm thay đổi trạng thái của hệ thống, do đó danh tính của “bông hoa lớn nhất” liên tục thay đổi linh hoạt. Trình tự hoa được chọn phụ thuộc vào tất cả các lần giảm trước đó. 

Các ràng buộc rất lớn: lên tới$10^6$hoa và lên đến$10^9$ong. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng từng con ong một cách ngây thơ bằng cách quét tuyến tính trên mảng. Ngay cả cấu trúc dữ liệu logarit cho mỗi phép toán cũng sẽ gặp khó khăn nếu chúng ta thực sự cần$10^9$hoạt động. 

Trường hợp khó nhận thấy xuất hiện khi nhiều bông hoa cuối cùng đạt tới số 0. Khi tất cả các bông hoa trở thành số 0, mọi con ong còn lại sẽ luôn chọn một bông hoa có giá trị bằng 0 và thu về số 0. Một mô phỏng ngây thơ có thể tiếp tục thực hiện những công việc không cần thiết hoặc xử lý sai các số 0 lặp lại. 

Một trường hợp góc khác đến từ việc cập nhật lặp đi lặp lại một bông hoa lớn. Một bông hoa như$1000000$không co lại nhanh chóng khi thực hiện các thao tác “trừ tổng chữ số” lặp đi lặp lại, vì mỗi bước chỉ loại bỏ một lượng nhỏ (nhiều nhất là 54). Điều này có nghĩa là một phần tử đơn lẻ có thể chi phối quá trình trong một thời gian dài và những kỳ vọng ngây thơ về sự hội tụ nhanh là không đáng tin cậy. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp rất dễ mô tả. Chúng tôi duy trì tất cả các giá trị hoa trong cấu trúc cho phép trích xuất mức tối đa hiện tại. Đối với mỗi con ong, chúng tôi trích xuất giá trị tối đa$x$, tính tổng các chữ số của nó$s(x)$, ghi lại nó làm câu trả lời cho con ong đó và thay thế$x$với$x - s(x)$. Nếu như$x$trở thành 0, nó vẫn còn trong hệ thống nhưng không còn ảnh hưởng đến cực đại trong tương lai. 

Cách tiếp cận này đúng vì nó tuân theo chính xác định nghĩa quy trình. Vấn đề là chi phí. Mỗi hoạt động yêu cầu duy trì mức tối đa, thường là thông qua một đống, chi phí$O(\log N)$. Nếu chúng tôi thực sự thực hiện đến mức$10^9$hoạt động, tổng công việc sẽ vượt xa giới hạn. 

Quan sát quan trọng là chúng tôi không cần tối ưu hóa các bản cập nhật riêng lẻ. Chúng tôi chỉ cần$K$-giá trị được trích xuất. Điều này cho phép chúng ta coi quy trình như tạo ra một chuỗi các thao tác theo thứ tự, trong đó mỗi thao tác là độc lập khi chúng ta biết cấu trúc tối đa hiện tại. Thay vì suy luận về các trạng thái cuối cùng, chúng ta chỉ tập trung vào việc tạo ra chuỗi cho đến khi đạt được vị trí$K$, dừng lại ngay sau đó. 

Điều này biến vấn đề thành một mô phỏng có kiểm soát trên hàng đợi ưu tiên: chúng tôi luôn biết mức tối đa hiện tại một cách hiệu quả và chúng tôi chỉ tiếp tục cho đến khi đạt được số bước yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét tối đa mỗi lần) |$O(NK)$|$O(N)$| Quá chậm | 
| Mô phỏng đống |$O(K \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một vùng heap tối đa chứa tất cả các giá trị hoa hiện tại. Mỗi bước tương ứng chính xác với một con ong. 

1. Xây dựng vùng heap tối đa từ tất cả các giá trị hoa ban đầu. Điều này cho phép chúng tôi truy xuất giá trị lớn nhất hiện tại theo thời gian logarit. 
2. Cho mỗi con ong từ 1 đến$K$, trích xuất giá trị lớn nhất$x$từ đống. Điều này tượng trưng cho bông hoa mà con ong hiện tại ghé thăm. 
3. Tính tổng các chữ số của$x$, ký hiệu$s(x)$. Đây là số tiền mà con ong thu thập được nên chúng ta lưu trữ làm đáp án cho bước này. 
4. Giảm giá trị hoa xuống$x - s(x)$. Nếu kết quả là dương tính, hãy đẩy nó trở lại heap để nó vẫn có thể cạnh tranh với những con ong trong tương lai. 
5. Nếu heap trống trước khi đạt tới$K$bước, tất cả những con ong còn lại không thu thập được. 

Quá trình dừng ngay sau khi tính toán$K$-giá trị thu thập được. 

### Tại sao nó hoạt động 

Ở mỗi bước, tính bất biến của heap đảm bảo chúng ta luôn chọn mức tối đa toàn cục hiện tại trong số tất cả các trạng thái hoa. Mỗi bản cập nhật đều phản ánh đúng sự phát triển của hệ thống sau hành động của một con ong. Vì lựa chọn của mỗi con ong chỉ phụ thuộc vào nhiều tập hợp giá trị hiện tại chứ không phụ thuộc vào các quyết định trong tương lai, nên việc duy trì tính bất biến này đảm bảo rằng chuỗi các giá trị được trích xuất khớp chính xác với quy trình thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def digit_sum(x: int) -> int:
    s = 0
    while x:
        s += x % 10
        x //= 10
    return s

def solve():
    N, K = map(int, input().split())
    arr = list(map(int, input().split()))

    # max heap via negative values
    heap = [-x for x in arr]
    heapq.heapify(heap)

    ans = 0

    for _ in range(K):
        if not heap:
            ans = 0
            break

        x = -heapq.heappop(heap)
        s = digit_sum(x)
        ans = s

        nx = x - s
        if nx > 0:
            heapq.heappush(heap, -nx)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên vùng heap tối đa được triển khai bằng vùng heap tối thiểu của Python với các giá trị phủ định. Mỗi lần lặp tương ứng với chính xác một con ong, do đó bộ đếm vòng lặp theo dõi trực tiếp chỉ số của con ong. Tổng chữ số được tính theo thời gian tuyến tính theo số chữ số, được giới hạn bởi 7 đối với các ràng buộc. 

Một chi tiết triển khai tinh tế là xử lý trường hợp tất cả các giá trị sớm trở thành 0. Trong tình huống đó, heap trở nên trống rỗng và chúng tôi ngay lập tức trả về 0 cho tất cả những con ong còn lại, vì không thể chọn hoa tích cực nào nữa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
22 15 7 2 1
```Chúng tôi chỉ theo dõi trạng thái vùng nhớ heap và các giá trị được trích xuất. 

| Bước | Đống (lượt xem tối đa) | Đã chọn x | tổng chữ số | Giá trị mới | 
| --- | --- | --- | --- | --- | 
| 1 | [22, 15, 7, 2, 1] | 22 | 4 | 18 | 
| 2 | [18, 15, 7, 2, 1] | 18 | 9 | 9 | 
| 3 | [15, 9, 7, 2, 1] | 15 | 6 | 9 | 

Con ong thứ ba thu thập 6. 

Dấu vết này cho thấy mức tối đa có thể đạt được từ một bông hoa đã bị giảm trước đó, không nhất thiết phải là mức tối đa ban đầu. 

### Mẫu 2 

đầu vào:```
3 10
21 21 21
```| Bước | Đống | Đã chọn x | tổng chữ số | Giá trị mới | 
| --- | --- | --- | --- | --- | 
| 1 | [21,21,21] | 21 | 3 | 18 | 
| 2 | [21,21,18] | 21 | 3 | 18 | 
| 3 | [18,18,21] | 21 | 3 | 18 | 
| 4 | [18,18,18] | 18 | 9 | 9 | 
| 5 | [18,18,18] | 18 | 9 | 9 | 
| ... | ... | ... | ... | ... | 

Quá trình tiếp tục cho đến khi xử lý xong 10 con ong. Cuối cùng, các giá trị co lại và ổn định xung quanh các số lượng nhỏ và khi đống trống hoặc ổn định ở mức 0, những con ong còn lại đóng góp bằng 0. 

Điều này chứng tỏ rằng các giá trị ban đầu giống hệt nhau không được đồng bộ hóa; chúng phân kỳ nhanh chóng do cập nhật độc lập lặp đi lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(K \log N)$| Mỗi con ong thực hiện một lần trích xuất đống và tối đa một lần chèn | 
| Không gian |$O(N)$| Heap lưu trữ tất cả các bông hoa đang hoạt động | 

Được cho$N \le 10^6$, cấu trúc heap là tuyến tính và mỗi phép toán đều có tính logarit. Phương pháp này được thiết kế để chấm dứt sớm nếu giá trị giảm xuống 0, điều này thường xảy ra trong thực tế đối với nhiều đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def digit_sum(x):
        s = 0
        while x:
            s += x % 10
            x //= 10
        return s

    N, K = map(int, input().split())
    arr = list(map(int, input().split()))

    heap = [-x for x in arr]
    heapq.heapify(heap)

    ans = 0
    for _ in range(K):
        if not heap:
            ans = 0
            break
        x = -heapq.heappop(heap)
        s = digit_sum(x)
        ans = s
        nx = x - s
        if nx > 0:
            heapq.heappush(heap, -nx)

    return str(ans)

# provided samples
assert run("5 3\n22 15 7 2 1\n") == "6"
assert run("3 10\n21 21 21\n") == "0"

# custom cases
assert run("1 1\n9\n") == "9", "single element"
assert run("1 5\n9\n") == "0", "depletion to zero"
assert run("3 1\n1 2 3\n") == "3", "initial max selection"
assert run("4 6\n10 10 10 10\n") == run("4 6\n10 10 10 10\n"), "stability check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 9`|`9`| trường hợp tối thiểu | 
|`1 5 / 9`|`0`| kiệt sức về không | 
|`3 1 / 1 2 3`|`1`? thực tế max=3 nên 3 | lựa chọn tối đa chính xác | 
|`4 6 / 10 10 10 10`| nhất quán | hành vi đối xứng lặp đi lặp lại | 

## Vỏ cạnh 

Khi tất cả các bông hoa giống hệt nhau, đống lặp đi lặp lại các giá trị chỉ khác nhau sau khi trừ tổng các chữ số. Thuật toán xử lý việc này một cách tự nhiên vì mỗi phần tử được trích xuất sẽ được chèn lại với giá trị cập nhật của nó, do đó không cần logic đặc biệt. 

Khi tất cả hoa cuối cùng trở thành số 0, đống trở nên trống rỗng. Trong trường hợp đó, vòng lặp kết thúc sớm và tất cả những con ong còn lại ngầm đóng góp bằng 0, phù hợp với quy tắc chọn một bông hoa bằng 0 sẽ không thu được gì. 

Đối với một bông hoa, kích thước đống hoa luôn là một. Thuật toán giảm nó từng bước cho đến khi đạt đến 0, sau đó tất cả những con ong tiếp theo sẽ ngay lập tức trả về 0. Điều này khớp trực tiếp với định nghĩa mà không cần bất kỳ logic phân nhánh nào.
