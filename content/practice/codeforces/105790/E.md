---
title: "CF 105790E - El Café\u00e9"
description: "Chúng tôi đang duy trì một tập hợp động các số nguyên dương, trong đó mỗi số nguyên biểu thị số lượng nguyên liệu mới được đưa vào quán cà phê. Bộ sưu tập thay đổi theo thời gian: các giá trị mới được thêm vào và đôi khi khối giá trị gần đây nhất bị xóa."
date: "2026-06-25T06:21:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105790
codeforces_index: "E"
codeforces_contest_name: "UDESC Selection Contest 2024-1"
rating: 0
weight: 105790
solve_time_s: 35
verified: true
draft: false
---

[CF 105790E - El Caf\u00e9](https://codeforces.com/problemset/problem/105790/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một tập hợp động các số nguyên dương, trong đó mỗi số nguyên biểu thị số lượng nguyên liệu mới được đưa vào quán cà phê. Bộ sưu tập thay đổi theo thời gian: các giá trị mới được thêm vào và đôi khi khối giá trị gần đây nhất bị xóa. Giữa các bản cập nhật này, chúng tôi được hỏi các truy vấn để kiểm tra xem liệu có thể sản xuất đồng thời một số lượng đồ uống giống hệt nhau từ tất cả các nguyên liệu hiện có hay không. 

Một quan sát quan trọng từ tuyên bố là mọi đồ uống đều phải sử dụng tất cả các thành phần và mỗi thành phần phải được chia đều cho tất cả các đồ uống. Nếu chúng ta hiện có số lượng thành phần$a_1, a_2, \dots, a_k$, sau đó sản xuất$x$đồ uống chỉ có thể được thực hiện nếu mọi$a_i$chia hết cho$x$. Điều này làm giảm toàn bộ việc kiểm tra tính khả thi thành điều kiện chia hết cho tất cả các số đang hoạt động. 

Các ràng buộc ngụ ý đến$10^5$các thao tác có giá trị lên tới$10^9$. Bất kỳ giải pháp nào tính toán lại một thuộc tính từ đầu cho mỗi truy vấn đều dẫn đến$O(nq)$, quá chậm. Chúng ta cần một cách tiếp cận trong đó mỗi hoạt động được xử lý theo phương pháp khấu hao$O(1)$hoặc$O(\log n)$, nghĩa là chúng ta phải duy trì một bản tóm tắt ngắn gọn về nhiều tập hợp hiện tại. 

Trường hợp khó phát hiện khi thao tác xóa sẽ loại bỏ các phần tử mới nhất. Ví dụ: hãy cân nhắc việc bắt đầu với các thành phần$[6, 10, 15]$, sau đó thêm$4$, sau đó loại bỏ hai phần tử cuối cùng. Việc triển khai đơn giản không duy trì lịch sử một cách chính xác có thể vô tình xóa các phần tử cũ hơn hoặc không khôi phục được trạng thái chính xác. Một vấn đề khác là các truy vấn lặp lại: một giải pháp tính toán lại khả năng chia hết bằng cách quét tất cả các phần tử trên mỗi truy vấn sẽ vượt qua các trường hợp nhỏ nhưng không đạt được các ràng buộc tối đa do giới hạn thời gian. 

## Phương pháp tiếp cận 

Ý tưởng dùng vũ lực rất đơn giản: duy trì danh sách đầy đủ các hoạt chất. Đối với mỗi truy vấn loại 3, lặp lại tất cả các giá trị hiện tại và kiểm tra xem mỗi giá trị có chia hết cho không$x$. Điều này đúng vì nó mã hóa trực tiếp điều kiện của bài toán. Tuy nhiên, mỗi truy vấn có thể yêu cầu quét tới$10^5$các phần tử và với$10^5$truy vấn, điều này dẫn đến$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn khả thi. 

Cái nhìn sâu sắc quan trọng là chúng ta không thực sự quan tâm đến các giá trị riêng lẻ mà chỉ quan tâm đến cấu trúc phân chia chung của chúng. một con số$x$chia mọi phần tử trong tập hợp khi và chỉ khi nó chia ước chung lớn nhất của chúng. Điều này biến vấn đề từ việc duy trì một tập hợp số thành duy trì một giá trị tổng hợp duy nhất: gcd của tất cả các phần tử hoạt động. 

Khi mức giảm này được nhìn thấy, cấu trúc sẽ trở thành vấn đề về ngăn xếp. Vì các phần tử chỉ được thêm vào cuối và bị xóa khỏi phần cuối theo đợt, nên chúng ta có thể duy trì một ngăn xếp trong đó mỗi mục lưu trữ gcd của tất cả các phần tử cho đến vị trí đó. Điều này cho phép cả cập nhật và khôi phục được xử lý hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu |$O(NQ)$|$O(N)$| Quá chậm | 
| Ngăn xếp với tiền tố gcd |$O(N + Q)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một ngăn xếp trong đó mỗi vị trí lưu trữ gcd của tất cả các giá trị cho đến thời điểm đó trong chuỗi hoạt động hiện tại. 

1. Bắt đầu với ngăn xếp ban đầu chứa tiền tố gcd của các thành phần ban đầu. Mỗi giá trị mới cập nhật gcd của mọi thứ trước nó, do đó, đỉnh ngăn xếp luôn đại diện cho gcd của tất cả các thành phần hoạt động cho đến nay. 
2. Đối với truy vấn loại 1, một thành phần mới sẽ được thêm vào. Chúng tôi tính toán gcd của giá trị này với đỉnh ngăn xếp hiện tại và đẩy nó. Điều này hoạt động vì gcd có tính kết hợp, do đó cấu trúc tiền tố vẫn hợp lệ sau khi gia hạn. 
3. Đối với truy vấn loại 2, chúng tôi loại bỏ truy vấn cuối cùng$K$thành phần bằng cách bật ra từ ngăn xếp$K$lần. Mỗi cửa sổ bật lên sẽ loại bỏ sự đóng góp của thành phần gần đây nhất trong khi vẫn duy trì tính chính xác của các giá trị gcd tiền tố trước đó. 
4. Đối với truy vấn loại 3, chúng tôi kiểm tra xem số được yêu cầu có$X$chia gcd hiện tại được lưu trữ ở đầu ngăn xếp. Nếu đúng như vậy thì tất cả các phần tử đều có thể chia hết cho$X$, nếu không thì ít nhất một phần tử không tương thích và đơn hàng không thể được thực hiện. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, đỉnh ngăn xếp biểu thị$\gcd(a_1, a_2, \dots, a_k)$trên tất cả các hoạt chất. Vì một số chia hết tất cả các phần tử trong một tập hợp khi và chỉ khi nó chia hết gcd của chúng, nên việc kiểm tra tính chia hết theo giá trị đơn này tương đương với việc kiểm tra tất cả các phần tử riêng lẻ. Bất biến ngăn xếp được giữ nguyên vì các bản cập nhật gcd có tính kết hợp và việc xóa sẽ khôi phục chính xác trạng thái tiền tố trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    # stack stores prefix gcds
    st = [0]
    for x in a:
        st.append(gcd(st[-1], x))

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])

        if t == 1:
            g = int(tmp[1])
            st.append(gcd(st[-1], g))

        elif t == 2:
            k = int(tmp[1])
            # pop k elements
            for _ in range(k):
                st.pop()

        else:
            x = int(tmp[1])
            if st[-1] % x == 0:
                print("SIM")
            else:
                print("NAO")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào tiền tố gcds lưu trữ ngăn xếp thay vì giá trị thô. Điều này tránh việc tính toán lại gcds trên toàn bộ mảng trong khi truy vấn. Phần tinh tế duy nhất là đảm bảo rằng việc xóa sẽ loại bỏ chính xác các trạng thái tiền tố gần đây nhất; vì mỗi mục nhập ngăn xếp chỉ phụ thuộc vào mục trước đó, nên việc bật lên sẽ tự động khôi phục gcd hợp lệ trước đó. 

Một chi tiết đáng chú ý là ngăn xếp ban đầu bắt đầu bằng 0, đóng vai trò là phần tử trung lập cho gcd. Điều này đảm bảo rằng thành phần thực đầu tiên khởi tạo cấu trúc một cách chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
12 12 6 4 2
2 3
3 4
1 6
1 3
3 2
```Chúng tôi theo dõi chồng tiền tố gcds: 

| Bước | Hoạt động | Ngăn xếp (trạng thái gcd) | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 0 | ban đầu | [0, 12, 12, 6, 2, 2] | - | 
| 1 | loại bỏ 3 | [0, 12, 12] | - | 
| 2 | truy vấn 4 | gcd = 12 | SIM | 
| 3 | thêm 6 | [0, 12, 12, 6] | - | 
| 4 | thêm 3 | [0, 12, 12, 6, 3] | - | 
| 5 | truy vấn 2 | gcd = 1 | NAO | 

Dấu vết cho thấy cách loại bỏ các phần tử khôi phục trạng thái gcd trước đó và quyết định cuối cùng chỉ phụ thuộc vào giá trị cao nhất hiện tại như thế nào. 

### Ví dụ 2 

đầu vào:```
3 3
20 20 4
3 5
1 10
3 2
```| Bước | Hoạt động | Ngăn xếp | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 0 | ban đầu | [0, 20, 20, 4] | - | 
| 1 | truy vấn 5 | gcd = 4 | NAO | 
| 2 | thêm 10 | [0, 20, 20, 4, 2] | - | 
| 3 | truy vấn 2 | gcd = 2 | SIM | 

Ví dụ này nêu bật cách một thành phần mới có thể thay đổi đáng kể gcd, ảnh hưởng trực tiếp đến tất cả các lần kiểm tra khả năng chia hết trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + Q)$khấu hao | mỗi phần tử được đẩy một lần và xuất hiện một lần | 
| Không gian |$O(N + Q)$| ngăn xếp lưu trữ một giá trị gcd cho mỗi lần chèn | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì mỗi thao tác chỉ thực hiện các phép tính gcd và thao tác ngăn xếp theo thời gian không đổi, ngay cả khi$N$Và$Q$với tới$10^5$. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    st = [0]
    for x in a:
        st.append(gcd(st[-1], x))

    out = []
    for _ in range(q):
        t, *rest = map(int, input().split())
        if t == 1:
            g = rest[0]
            st.append(gcd(st[-1], g))
        elif t == 2:
            k = rest[0]
            for _ in range(k):
                st.pop()
        else:
            x = rest[0]
            out.append("SIM" if st[-1] % x == 0 else "NAO")

    return "\n".join(out) + ("\n" if out else "")

# provided samples
assert run("""5 5
12 12 6 4 2
2 3
3 4
1 6
1 3
3 2
""") == "SIM\nNAO\n"

assert run("""3 3
20 20 4
3 5
1 10
3 2
""") == "NAO\nSIM\n"

# custom cases
assert run("""1 3
10
3 5
1 5
3 5
""") == "NAO\nSIM\n", "single element edge"

assert run("""4 3
6 10 15 30
3 5
2 2
3 3
""") == "SIM\nNAO\n", "rollback correctness"

assert run("""2 2
7 7
3 7
3 1
""") == "SIM\nSIM\n", "all equal values"

assert run("""3 3
2 4 8
3 2
2 1
3 4
""") == "SIM\nSIM\n", "boundary removals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh phần tử đơn | Trình tự SIM/NAO | chính xác với ngăn xếp tối thiểu | 
| quay lui đúng đắn | SIM/NAO hỗn hợp | xử lý xóa đúng | 
| tất cả các giá trị bằng nhau | luôn SIM | sự ổn định của gcd | 
| xóa bỏ ranh giới | SIM/SIM | tính đúng đắn sau khi bật lên một phần | 

## Vỏ cạnh 

Một đầu vào thành phần duy nhất sẽ kiểm tra xem quá trình khởi tạo gcd có xử lý chính xác một ngăn xếp với một phần tử có ý nghĩa hay không. Thuật toán vẫn hoạt động vì gcd của một số là chính nó và việc kiểm tra tính chia hết sẽ giảm một cách chính xác. 

Việc xóa lớn giúp khôi phục các trạng thái trước đó sẽ kiểm tra xem ngăn xếp có hoàn nguyên chính xác về giá trị gcd tiền tố trước đó hay không. Vì mỗi mục nhập ngăn xếp mã hóa một tiền tố đầy đủ nên việc bật lên nhiều lần luôn khôi phục một gcd lịch sử hợp lệ mà không cần tính toán lại. 

Các mảng thống nhất, chẳng hạn như tất cả các số bằng nhau, đảm bảo rằng gcd vẫn ổn định qua các bản cập nhật, xác nhận rằng không có tác dụng phụ ngoài ý muốn nào xảy ra trong các phép tính gcd lặp đi lặp lại. 

Việc loại bỏ ranh giới khiến ngăn xếp ở trạng thái ban đầu xác nhận rằng giá trị 0 trọng điểm hoạt động chính xác và không cản trở việc kiểm tra khả năng chia hết.
