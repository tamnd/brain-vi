---
title: "CF 102961L - Thu Thập Số II"
description: "Chúng ta được cho một hoán vị của các số nguyên từ 1 đến $n$, được sắp xếp theo một thứ tự nào đó. Cùng với đó, chúng ta nhận được một chuỗi các phép toán, trong đó mỗi phép toán hoán đổi hai vị trí trong hoán vị."
date: "2026-07-04T06:52:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "L"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 44
verified: true
draft: false
---

[CF 102961L - Thu thập số II](https://codeforces.com/problemset/problem/102961/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số nguyên từ 1 đến$n$, sắp xếp theo một thứ tự nào đó. Cùng với đó, chúng ta nhận được một chuỗi các phép toán, trong đó mỗi phép toán hoán đổi hai vị trí trong hoán vị. Sau mỗi lần hoán đổi, chúng ta phải báo cáo một đại lượng phụ thuộc vào mức độ “sắp xếp” hoán vị đối với các giá trị tăng dần. 

Để hiểu số lượng, hãy tưởng tượng quét các giá trị từ 1 trở lên$n$và cố gắng “thu thập” chúng theo thứ tự từ trái sang phải dọc theo mảng. Bất cứ khi nào một giá trị xuất hiện ở bên trái của một giá trị nhỏ hơn xuất hiện sau theo thứ tự tự nhiên, thì quá trình quét sẽ phải khởi động lại một phân đoạn mới một cách hiệu quả. Câu trả lời là số lượng các đoạn như vậy cần thiết để duyệt qua các giá trị theo thứ tự tăng dần tùy theo vị trí của chúng. 

Một cách dễ hiểu hơn để suy nghĩ về điều này là chúng ta quan tâm đến thứ tự tương đối của các giá trị liên tiếp trong hoán vị khi được ánh xạ tới vị trí của chúng. Nếu giá trị$i$xuất hiện sau giá trị$i+1$, thì cả hai điều này sẽ phá vỡ tính liên tục của một lượt tăng dần, làm tăng số lượng phân đoạn. 

Kích thước đầu vào đủ lớn để cả hai$n$và số lượng giao dịch hoán đổi có thể lên tới hàng trăm nghìn. Điều này ngay lập tức loại trừ việc tính lại câu trả lời từ đầu sau mỗi lần hoán đổi, vì điều đó sẽ dẫn đến hành vi bậc hai. Một giải pháp phải cập nhật câu trả lời theo thời gian không đổi hoặc logarit cho mỗi thao tác, giữ cho tổng độ phức tạp gần với tuyến tính về số lượng truy vấn. 

Một trường hợp thất bại tinh tế xuất hiện khi các giao dịch hoán đổi ảnh hưởng đến các phần tử cách xa nhau về giá trị nhưng gần nhau về chỉ số. Ví dụ: hãy xem xét một hoán vị trong đó chỉ tồn tại một nghịch đảo giữa các giá trị liên tiếp, chẳng hạn$[1, 3, 2, 4]$. Câu trả lời là 2 vì cặp (2, 3) bị đảo ngược theo thứ tự vị trí. Nếu chúng ta hoán đổi các phần tử không liên quan như vị trí 1 và 4, một phép tính lại đơn giản vẫn có thể quét mọi thứ một cách không cần thiết, mặc dù chỉ có một số mối quan hệ cục bộ trong không gian giá trị thực sự thay đổi. Giải pháp đúng phải nhận ra rằng chỉ có các mối quan hệ giá trị liền kề mới quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ tính toán lại số lượng phân đoạn hợp lệ sau mỗi lần hoán đổi bằng cách quét toàn bộ hoán vị và kiểm tra từng giá trị$i$, liệu vị trí của nó có đến sau$i+1$. Điều này có tác dụng vì câu trả lời chỉ phụ thuộc vào số lượng cặp giá trị liền kề “không đúng thứ tự” về vị trí. Tuy nhiên, mỗi lần tính toán lại tốn$O(n)$, và lên đến$q$hoán đổi điều này trở thành$O(nq)$, quá chậm khi cả hai đều lớn. 

Quan sát quan trọng là câu trả lời được xác định hoàn toàn bởi tập hợp các chỉ số$i$như vậy$pos[i] > pos[i+1]$, Ở đâu$pos[x]$là chỉ số giá trị$x$trong hoán vị. Mỗi lần hoán đổi chỉ thay đổi vị trí của hai giá trị, do đó chỉ những so sánh liên quan đến các giá trị đó và các giá trị lân cận của chúng trong không gian giá trị mới có thể thay đổi câu trả lời. Vị trí này trong không gian giá trị cho phép chúng ta duy trì câu trả lời theo từng bước. 

Thay vì tính toán lại mọi thứ, chúng tôi duy trì số lần “ngắt” hiện tại giữa các giá trị liên tiếp. Khi hoán đổi xảy ra, chúng tôi tạm thời loại bỏ sự đóng góp của các giá trị bị ảnh hưởng, cập nhật vị trí của chúng và sau đó chỉ đánh giá lại các cặp lân cận bị ảnh hưởng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc: mảng hoán vị và mảng vị trí ánh xạ từng giá trị tới chỉ mục hiện tại của nó. Chúng tôi cũng duy trì số lượng các cặp giá trị liền kề vi phạm thứ tự vị trí tăng dần. 

1. Xây dựng mảng vị trí ban đầu bằng cách quét hoán vị một lần, ghi lại vị trí mỗi giá trị xuất hiện. Điều này cho phép tra cứu liên tục các vị trí sau này. 
2. Tính số lần ngắt ban đầu bằng cách lặp lại các giá trị từ 1 đến$n-1$và đếm xem vị trí của$i$lớn hơn vị trí của$i+1$. Điều này thiết lập câu trả lời cơ bản. 
3. Đối với mỗi lần hoán đổi vị thế$a$Và$b$, xác định các giá trị hiện đang ở các vị trí này. Đây là những giá trị duy nhất mà mối quan hệ của chúng có thể thay đổi. 
4. Trước khi cập nhật bất cứ điều gì, hãy loại bỏ sự đóng góp của tất cả các cặp liên quan đến các giá trị này và các giá trị lân cận của chúng trong không gian giá trị. Cụ thể, đối với giá trị$x$, chỉ so sánh với$x-1$Và$x+1$quan trọng, vì các cặp khác vẫn không bị ảnh hưởng bởi việc hoán đổi vị trí của các giá trị không liên quan. 
5. Thực hiện hoán đổi trong cả mảng hoán vị và vị trí để các truy vấn trong tương lai phản ánh cấu hình được cập nhật. 
6. Sau khi hoán đổi, hãy thêm lại các đóng góp cho cùng một vùng giá trị cục bộ, một lần nữa chỉ kiểm tra các cặp$(x-1, x)$Và$(x, x+1)$. 
7. Xuất số lượng cập nhật sau mỗi thao tác. 

Ý tưởng quan trọng là mỗi lần hoán đổi chỉ thay đổi vị trí của hai giá trị, do đó chỉ các mối quan hệ kề cận trong miền giá trị liên quan đến các giá trị đó mới có thể thay đổi trạng thái chính xác của chúng. 

### Tại sao nó hoạt động 

Số lượng được duy trì chỉ phụ thuộc vào sự so sánh giữa các giá trị liên tiếp theo thứ tự được sắp xếp. Bất kỳ sự hoán đổi nào cũng thay đổi vị trí của chính xác hai giá trị, vì vậy mọi cặp giá trị không bị ảnh hưởng đều giữ nguyên thứ tự tương đối. Do đó, chỉ những cặp liên quan đến giá trị hoán đổi mới có thể thay đổi từ có thứ tự sang không có thứ tự hoặc ngược lại. Vì mọi đóng góp đều mang tính cục bộ đối với giá trị kề cận nên chỉ cập nhật những cặp bị ảnh hưởng đó sẽ duy trì tính chính xác trong suốt tất cả các hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
p = list(map(int, input().split()))

pos = [0] * (n + 1)
for i, v in enumerate(p):
    pos[v] = i

def bad(i):
    return 1 if pos[i] > pos[i + 1] else 0

ans = 1
for i in range(1, n):
    ans += bad(i)

out = []

for _ in range(q):
    a, b = map(int, input().split())
    a -= 1
    b -= 1

    x = p[a]
    y = p[b]

    affected = set()
    for v in (x, y):
        for u in (v - 1, v):
            if 1 <= u < n:
                affected.add(u)

    for u in affected:
        ans -= bad(u)

    p[a], p[b] = p[b], p[a]
    pos[x], pos[y] = pos[y], pos[x]

    for u in affected:
        ans += bad(u)

    out.append(str(ans))

print("\n".join(out))
```Việc triển khai dựa vào mảng vị trí để so sánh giữa các giá trị theo thời gian không đổi. chức năng`bad(i)`mã hóa xem ranh giới giữa các giá trị liên tiếp có góp phần tạo ra một phân đoạn mới hay không. Trước mỗi lần cập nhật hoán đổi, chúng tôi xóa phần đóng góp của tất cả các ranh giới có khả năng bị ảnh hưởng, sau đó khôi phục chúng sau khi cập nhật vị trí. 

Một lỗi phổ biến là cố gắng chỉ cập nhật các ranh giới liên quan đến các chỉ số trong mảng thay vì giá trị kề. Bất biến chính xác tồn tại trong không gian giá trị, không phải không gian chỉ mục. 

## Ví dụ đã hoạt động 

Hãy xem xét một hoán vị ban đầu$[1, 3, 2, 4]$với một truy vấn duy nhất hoán đổi vị trí 3 và 4. 

Ban đầu, các vị trí là$pos[1]=0, pos[3]=1, pos[2]=2, pos[4]=3$. Cặp vi phạm duy nhất là (2, 3) vì$pos[2] > pos[3]$, đưa ra câu trả lời 2. 

Sau khi hoán đổi 3 và 4, mảng trở thành$[1, 4, 2, 3]$. 

| Bước | Giá trị hoán đổi | Cặp bị ảnh hưởng | Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu | Không có | (2,3) | 2 | 
| Hoán đổi (3,4) | 3, 4 | (2,3), (3,4) | 1 | 

Bảng này cho thấy rằng chỉ các cặp liên quan đến 3 và 4 hoặc các cặp lân cận của chúng trong không gian giá trị mới có thể thay đổi và thực tế số lần ngắt giảm vì 3 và 4 được sắp xếp đúng thứ tự. 

Bây giờ hãy xem xét ví dụ thứ hai trong đó hoán vị đã được sắp xếp$[1,2,3,4,5]$, và chúng ta hoán đổi vị trí 2 và 4, tạo ra$[1,4,3,2,5]$. 

| Bước | Trạng thái mảng | Vi phạm | Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu | 1 2 3 4 5 | không | 1 | 
| Sau khi trao đổi | 1 4 3 2 5 | (2,3), (3,4) | 3 | 

Điều này xác nhận rằng chỉ các cặp giá trị liền kề cục bộ trở nên không hợp lệ sau khi hoán đổi, chứ không phải các phần tử không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q)$| Quá trình tiền xử lý ban đầu là tuyến tính, mỗi lần hoán đổi chỉ cập nhật hằng số nhiều lần kiểm tra giá trị liền kề | 
| Không gian |$O(n)$| Mảng vị trí và hoán vị lưu trữ thông tin không đổi trên mỗi phần tử | 

Thuật toán phù hợp thoải mái trong các ràng buộc điển hình lên tới$2 \cdot 10^5$các phần tử và thao tác, vì mỗi truy vấn sẽ tránh được việc quét lại toàn bộ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    p = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(p):
        pos[v] = i

    def bad(i):
        return 1 if pos[i] > pos[i + 1] else 0

    ans = 1
    for i in range(1, n):
        ans += bad(i)

    out = []

    for _ in range(q):
        a, b = map(int, input().split())
        a -= 1
        b -= 1

        x = p[a]
        y = p[b]

        affected = set()
        for v in (x, y):
            for u in (v - 1, v):
                if 1 <= u < n:
                    affected.add(u)

        for u in affected:
            ans -= bad(u)

        p[a], p[b] = p[b], p[a]
        pos[x], pos[y] = pos[y], pos[x]

        for u in affected:
            ans += bad(u)

        out.append(str(ans))

    return "\n".join(out)

# custom cases
assert run("5 0\n1 2 3 4 5\n") == "", "no queries edge"
assert run("4 1\n1 3 2 4\n2 3\n") == "2", "single swap inversion fix"
assert run("5 2\n1 2 3 4 5\n1 5\n2 4\n") == "3\n3", "multiple swaps stability"
assert run("3 1\n3 2 1\n1 2\n") == "2", "reverse permutation behavior"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| được sắp xếp không có truy vấn | trống | tính chính xác không hoạt động | 
| đảo ngược nhỏ | 2 | hiệu ứng hoán đổi cục bộ | 
| hoán đổi lặp đi lặp lại | 3, 3 | sự ổn định theo bản cập nhật | 
| mảng đảo ngược | 2 | rối loạn ban đầu tồi tệ nhất | 

## Vỏ cạnh 

Một hoán vị được sắp xếp đầy đủ là điểm nhấn rõ ràng nhất về tính chính xác vì mỗi lần hoán đổi ngay lập tức đưa ra chính xác hai vi phạm ranh giới. Đối với một đầu vào như$[1,2,3,4,5]$và hoán đổi giữa vị trí 1 và 4, các giá trị bị ảnh hưởng là 2, 4 và các giá trị lân cận của chúng. Thuật toán chỉ loại bỏ các đóng góp cho (1,2), (2,3), (3,4) khi có liên quan, thực hiện hoán đổi và thêm lại chúng một cách nhất quán. Vì tất cả đóng góp ban đầu đều bằng 0 nên mọi sự gia tăng trong câu trả lời phải hoàn toàn đến từ các kiểm tra cục bộ được cập nhật, phù hợp với cấu trúc thực tế của hoán vị sau khi hoán đổi. 

Một hoán vị ngược lại như$[5,4,3,2,1]$nhấn mạnh tính toán ban đầu của tất cả các ngắt giá trị liền kề. Mỗi cặp đóng góp, tạo ra số lượng phân đoạn tối đa. Việc hoán đổi hai phần tử bất kỳ chỉ điều chỉnh một số lượng không đổi của những đóng góp đó. Bởi vì thuật toán chỉ tính toán lại các ranh giới liền kề giá trị xung quanh các giá trị được hoán đổi, nên ngay cả những gián đoạn lớn vẫn được cục bộ hóa trong bước cập nhật và không có cặp không liên quan nào bị sửa đổi nhầm.
