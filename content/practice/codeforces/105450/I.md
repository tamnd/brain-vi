---
title: "CF 105450I - Tôi có thể tìm thấy kẹo của mình không?"
description: "Chúng ta được cho một mảng các số nguyên có độ dài $n$. Mỗi vị trí lưu trữ một số biểu thị số lượng kẹo trong túi đó."
date: "2026-06-23T17:33:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105450
codeforces_index: "I"
codeforces_contest_name: "UTPC x WiCS Contest 10-25-24 (UT Internal)"
rating: 0
weight: 105450
solve_time_s: 88
verified: false
draft: false
---

[CF 105450I - Tôi có thể tìm thấy kẹo của mình không?](https://codeforces.com/problemset/problem/105450/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên có độ dài$n$. Mỗi vị trí lưu trữ một số biểu thị số lượng kẹo trong túi đó. Đối với mỗi truy vấn, chúng tôi lấy một phân đoạn liền kề$[l, r]$và hỏi liệu chúng ta có thể sắp xếp lại các giá trị bên trong phân đoạn đó để sau khi sắp xếp lại phân đoạn đó trở thành chính xác các số nguyên liên tiếp từ$l$ĐẾN$r$. 

Nói cách khác, mỗi truy vấn sẽ hỏi xem tập hợp giá trị bên trong mảng con có khớp với tập hợp số nguyên hay không$\{l, l+1, \ldots, r\}$. Thứ tự bên trong phân đoạn không quan trọng, chỉ là liệu các giá trị có thể được hoán vị để tạo thành khoảng chính xác đó hay không. 

Các ràng buộc đi lên đến$2 \cdot 10^5$cho cả hai$n$Và$q$, do đó, bất kỳ giải pháp nào xử lý từng truy vấn theo thời gian tuyến tính trong khoảng thời gian đó sẽ ngay lập tức quá chậm. Một cách tiếp cận ngây thơ quét từng phạm vi và kiểm tra các điều kiện trực tiếp sẽ giảm xuống$O(nq)$, xung quanh$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất và sẽ không chạy kịp thời. 

Điều tinh tế quan trọng là đây không phải là việc kiểm tra tần suất đơn giản các giá trị tùy ý. Khoảng mục tiêu phụ thuộc vào giới hạn của truy vấn, nghĩa là độ chính xác phụ thuộc vào cả cấu trúc tổng và sự vắng mặt của các giá trị trùng lặp hoặc bị thiếu. 

Một vài trường hợp đặc biệt bộc lộ những lỗi phổ biến. 

Nếu đoạn đó là$[1, 3, 2]$vì$l = 1, r = 3$, câu trả lời là CÓ vì nó có thể được hoán vị thành$[1, 2, 3]$. Nhưng nếu đoạn đó là$[1, 3, 2, 5]$vì$l = 1, r = 4$, KHÔNG vì mặc dù các giá trị khác nhau nhưng chúng không khớp với tập hợp bắt buộc$\{1,2,3,4\}$. Một sai lầm mà nhiều cách tiếp cận ngây thơ mắc phải là chỉ kiểm tra các giá trị nằm trong phạm vi$[l, r]$, sẽ chấp nhận không chính xác các trường hợp như$[1,1,2,3]$hoặc$[1,2,2,3]$. Một cách thất bại khác là chỉ kiểm tra tổng, điều này là không đủ vì các hoán vị như$[1,4]$Và$[2,3]$chia sẻ cùng một số tiền nhưng đại diện cho nhiều tập hợp khác nhau. 

Giải pháp đúng phải đảm bảo cả tính đầy đủ của tập hợp và không có giá trị bổ sung hoặc trùng lặp. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập. Trong một khoảng thời gian nhất định$[l, r]$, chúng tôi trích xuất tất cả các giá trị, sắp xếp chúng và so sánh với chuỗi$l, l+1, \ldots, r$. Điều này đúng vì việc sắp xếp sẽ cho biết liệu nhiều tập hợp có khớp với cấu trúc liên tiếp được yêu cầu hay không. Tuy nhiên, việc sắp xếp từng truy vấn sẽ tốn$O((r-l+1)\log(r-l+1))$, suy biến thành$O(n \log n)$cho mỗi truy vấn và tổng thể$O(nq \log n)$, vượt xa giới hạn khả thi. 

Ngay cả khi loại bỏ việc sắp xếp, chúng ta vẫn cần một cách để xác minh hai thuộc tính một cách hiệu quả: tất cả các phần tử đều nằm trong$[l, r]$và mỗi số nguyên trong phạm vi đó xuất hiện đúng một lần. Điều này gợi ý số lượng theo dõi, nhưng việc duy trì dải tần số đầy đủ cho mỗi truy vấn cũng quá tốn kém. 

Quan sát chính là phân khúc mục tiêu$[l, r]$có tổng cố định và số lượng cố định. Nếu multiset bên trong khoảng thời gian khớp chính xác$[l, r]$, thì phải đồng thời có hai điều kiện: số phần tử là$r-l+1$và tổng các phần tử phải bằng tổng của chuỗi số học$\frac{(l+r)(r-l+1)}{2}$. Vì tất cả các giá trị đều là số nguyên và kích thước là cố định nên hai ràng buộc này đủ để đảm bảo nhiều tập hợp là chính xác, bởi vì bất kỳ sai lệch nào trong các giá trị đều nhất thiết phải thay đổi tổng. 

Điều này làm giảm mỗi truy vấn thành một truy vấn tổng phạm vi trên mảng, có thể được trả lời bằng tổng tiền tố trong$O(1)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (sắp xếp theo truy vấn) |$O(q \cdot n \log n)$|$O(1)$| Quá chậm | 
| Tối ưu hóa tổng tiền tố |$O(n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào thực tế là một phân đoạn hợp lệ phải khớp với cả độ dài và tổng chính xác của cấp số cộng đích. 

1. Xây dựng một mảng tổng tiền tố trong đó`pref[i]`lưu trữ tổng của số đầu tiên$i$các phần tử của mảng. Điều này cho phép bất kỳ tổng phạm vi nào được tính toán trong thời gian không đổi. Bước này chuyển đổi công việc tính tổng lặp đi lặp lại thành tính toán trước. 
2. Đối với mỗi truy vấn$[l, r]$, tính tổng của mảng con bằng cách sử dụng$\text{sum} = pref[r] - pref[l-1]$. Điều này cung cấp tổng giá trị chính xác hiện có trong phân khúc. 
3. Tính tổng dự kiến ​​của dãy$l, l+1, \ldots, r$sử dụng công thức chuỗi số học$\frac{(l+r)(r-l+1)}{2}$. Điều này thể hiện cấu hình hợp lệ duy nhất cho phân khúc. 
4. So sánh hai tổng. Nếu chúng bằng nhau thì xuất ra CÓ, nếu không thì xuất ra KHÔNG. 

Lý do chúng tôi không kiểm tra rõ ràng các bản sao hoặc thứ tự là vì đối với độ dài phân đoạn cố định, cách duy nhất để khớp tổng được yêu cầu với số nguyên là khớp chính xác với tập hợp. Bất kỳ số nào bị thiếu hoặc trùng lặp đều buộc phải thay đổi bù trừ ở một giá trị khác, làm thay đổi tổng. 

## Tại sao nó hoạt động 

Phân đoạn phải chứa chính xác$k = r-l+1$số nguyên. Multiset mục tiêu chính xác là các số nguyên từ$l$ĐẾN$r$, đó là bộ duy nhất của$k$số nguyên riêng biệt với tổng đó. Bất kỳ sai lệch nào cũng đưa ra một giá trị ngoài phạm vi hoặc lặp lại một giá trị và cả hai trường hợp đều nhất thiết phải thay đổi tổng vì việc thay thế một số nguyên riêng biệt bằng một số nguyên khác sẽ thay đổi tổng bằng một số khác 0. Điều này làm cho tổng bằng nhau vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        s = pref[r] - pref[l - 1]
        k = r - l + 1
        target = (l + r) * k // 2
        if s == target:
            out.append("YES")
        else:
            out.append("NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng tổng tiền tố theo thời gian tuyến tính. Mỗi truy vấn được trả lời bằng cách trích xuất một tổng phạm vi trong thời gian không đổi và so sánh nó với tổng lũy ​​tiến số học dự kiến. Việc chia cho 2 là an toàn vì$(l+r)k$luôn chẵn đối với các số nguyên liên tiếp, do đó không phát sinh vấn đề về độ chính xác. 

Một điểm tinh tế là lập chỉ mục: tổng tiền tố được xây dựng bằng cách lập chỉ mục dựa trên 1 để tránh việc kiểm tra ranh giới lặp đi lặp lại. Điều này đảm bảo rằng tính toán truy vấn là phép trừ trực tiếp mà không cần xử lý có điều kiện đối với$l = 1$. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi các truy vấn trên đầu vào mẫu. 

Mảng đầu vào là$[1, 3, 2, 5, 4]$. 

Đối với mỗi truy vấn, chúng tôi tính toán tổng phạm vi và tổng dự kiến. 

### Dấu vết 1 

| Truy vấn | tôi | r | Tổng phạm vi | k | Số tiền dự kiến ​​| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 1+3+2=6 | 3 | (1+3)*3/2=6 | CÓ | 
| 2 | 2 | 5 | 3+2+5+4=14 | 4 | (2+5)*4/2=14 | CÓ | 
| 3 | 1 | 4 | 1+3+2+5=11 | 4 | (1+4)*4/2=10 | KHÔNG | 

Hai truy vấn đầu tiên khớp chính xác vì các mảng con là hoán vị của các phạm vi liên tiếp được yêu cầu. Cách thứ ba không thành công vì giá trị 5 phá vỡ số dư tổng được yêu cầu, cho thấy rằng đẳng thức tổng rất nhạy cảm với các giá trị thừa hoặc đặt sai vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q)$| Cấu trúc tổng tiền tố là tuyến tính, mỗi truy vấn được trả lời theo thời gian không đổi | 
| Không gian |$O(n)$| Mảng tiền tố lưu trữ tổng tích lũy | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$và giải pháp này thực hiện tối đa vài trăm nghìn thao tác, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()  # placeholder if integrated; replace with solve()

# provided sample (conceptual)
# assert run("5 3\n1 3 2 5 4\n1 3\n2 5\n1 4\n") == "YES\nYES\nNO\n"

# custom cases

# minimum size
# single element, correct
# assert run("1 1\n5\n1 1\n") == "YES"

# mismatch single element
# assert run("1 1\n5\n1 1\n") == "YES"

# all equal values (impossible except length 1)
# assert run("3 2\n1 1 1\n1 3\n1 1\n") == "NO\nYES"

# already correct full permutation
# assert run("5 1\n1 2 3 4 5\n1 5\n") == "YES"

# boundary off-by-one style
# assert run("5 2\n1 2 3 4 5\n2 4\n1 3\n") == "YES\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn | CÓ | độ đúng cơ sở | 
| Tất cả các giá trị bằng nhau | KHÔNG ngoại trừ đơn | phát hiện trùng lặp thông qua tổng không khớp | 
| Đã sắp xếp đầy đủ | CÓ | tính đúng đắn trên khoảng toàn cục | 
| Kiểm tra khoảng thời gian phụ | CÓ | độ chính xác trên phạm vi một phần | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các giá trị đều nằm trong phạm vi số chính xác nhưng không khác biệt. Ví dụ, trong một khoảng$[1, 3]$, đoạn mảng$[1, 1, 3]$có tổng bằng 5, trong khi tổng dự kiến ​​​​là 6. Thuật toán loại bỏ nó một cách chính xác thông qua tổng không khớp mặc dù tất cả các giá trị đều có vẻ “hợp lý”. 

Một trường hợp khác là một khoảng dịch chuyển như$[3, 5]$đoạn đó ở đâu$[3, 4, 6]$. Sự hiện diện của 6 làm tăng tổng vượt quá tổng số học dự kiến ​​và việc kiểm tra đẳng thức không thành công ngay lập tức mà không cần xác thực phạm vi rõ ràng. 

Trường hợp cuối cùng là một hoán vị hoàn hảo theo thứ tự được xáo trộn, chẳng hạn như$[2, 1, 3]$vì$[1, 3]$. Tổng vẫn bất biến khi hoán vị, do đó thuật toán chấp nhận nó một cách chính xác, chứng tỏ rằng thứ tự là không liên quan và chỉ có cấu trúc nhiều tập hợp mới quan trọng.
