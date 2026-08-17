---
title: "CF 102354I - Từ mô-đun đến hợp lý"
description: "Chúng ta cần khôi phục một số hữu tỉ dương (x=p/q), trong đó cả (p) và (q) nhiều nhất là (10^9). Chúng ta không thể nhìn thấy (p) và (q) một cách trực tiếp. Thay vào đó, đối với mô đun nguyên tố được chọn (m), giám khảo đưa ra số nguyên (r) trong phạm vi (0le r<m) thỏa mãn [ requiv p q^{-1}pmod m."
date: "2026-08-16T08:43:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "I"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 315
verified: false
draft: false
---

[CF 102354I - Từ mô-đun đến hợp lý](https://codeforces.com/problemset/problem/102354/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần khôi phục một số hữu tỉ dương (x=p/q), trong đó cả (p) và (q) nhiều nhất là (10^9). Chúng ta không thể nhìn thấy (p) và (q) một cách trực tiếp. Thay vào đó, đối với môđun nguyên tố được chọn (m), giám khảo đưa ra số nguyên (r) trong khoảng (0\le r<m) thỏa mãn 

[ 
r\equiv p q^{-1}\pmod m. 
] 

Vì (m) là số nguyên tố và (q<m) nên tồn tại nghịch đảo (q^{-1}). Tương tự, thông tin được truy vấn trả về chính xác là câu lệnh 

[ 
rq\equiv p\pmod m. 
] 

Nhiệm vụ này mang tính tương tác, do đó, đối với mọi trường hợp thử nghiệm, chương trình của chúng tôi sẽ in các truy vấn, đọc câu trả lời của giám khảo và cuối cùng in ra một cặp (p,q) đại diện cho số hữu tỷ ẩn. Cặp đôi không cần phải giảm. Ví dụ: (1/2), (2/4) và (500000000/1000000000) đều đại diện cho cùng một giá trị và bất kỳ giá trị nào trong số đó đều được chấp nhận. Tuyên bố chính thức chỉ định tối đa (10^5) trường hợp thử nghiệm và tối đa mười truy vấn cho mỗi trường hợp. 

Giới hạn (p,q\le10^9) là ràng buộc cấu trúc trung tâm. Việc tìm kiếm trực tiếp trên các mẫu số có thể yêu cầu tối đa (10^9) lần lặp cho một trường hợp thử nghiệm. Với (10^5) trường hợp, điều đó trở thành (10^{14}) phép tính mô-đun, vượt xa giới hạn sáu giây có thể hỗ trợ. Chúng ta cần biến thông tin mô-đun thành một bản tái cấu trúc hợp lý chính xác bằng cách sử dụng lý thuyết số logarit theo thời gian. 

Ngoài ra còn có một sự khác biệt tinh tế giữa tính độc đáo và sự tái thiết. Nếu bằng cách nào đó chúng ta biết giá trị theo mô đun (M>10^{18}), thì số hữu tỉ sẽ là duy nhất trong số tất cả các phân số có tử số và mẫu số nhiều nhất là (10^9). Thật vậy, nếu cả (p_1/q_1) và (p_2/q_2) đều tạo ra cùng một dư lượng thì 

[ 
p_1q_2\equiv p_2q_1\pmod M. 
] 

Do đó (M) chia hết (p_1q_2-p_2q_1). Giá trị tuyệt đối của chênh lệch đó lớn nhất là (10^{18}), do đó, mô đun lớn hơn (10^{18}) sẽ buộc chênh lệch bằng 0. Quan sát tính duy nhất này cũng là cơ sở của các giải pháp tiêu chuẩn cho vấn đề. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Đầu tiên là một phân số không rút gọn. Ví dụ: giá trị ẩn có thể là (2/4). Một đầu ra đúng là`1 2`, mặc dù mẫu chọn`2 4`. Việc triển khai nhấn mạnh vào việc khôi phục cặp gốc chính xác thay vì giá trị hợp lý sẽ từ chối câu trả lời hoàn toàn hợp lệ. 

Số thứ hai là một số nguyên như (2/1). Ở đây thặng dư mô-đun chỉ đơn giản là (2), vì vậy câu trả lời đúng có thể là`2 1`. Một quy trình xây dựng lại chỉ tìm kiếm mẫu số phân số tiếp tục không cần thiết có thể vô tình bỏ qua phần dư ban đầu và thất bại trong trường hợp này. 

Thứ ba là giá trị (1/1). Đối với mỗi mô đun được truy vấn, phần dư được trả về là (1). Việc triển khai CRT hoặc Euclide bất cẩn giả định phần dư lớn có thể xử lý sai hợp lý nhỏ nhất có thể này. Đầu ra đúng là`1 1`. 

Thứ tư là một phân số ở ranh giới số. Ví dụ: (1000000000/999999999) là hợp lệ ngay cả khi tử số và mẫu số của nó đều gần giới hạn trên. Việc tái tạo dấu phẩy động ở đây không an toàn vì các số nguyên liên quan có thể ở khoảng (10^{24}) sau CRT. Giải pháp phải sử dụng số học số nguyên chính xác xuyên suốt. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu bắt đầu từ sự phù hợp 

[ 
rq\equiv p\pmod M. 
] 

Với mọi (q) có thể có từ (1) đến (10^9), chúng ta có thể tính được 

[ 
p=(rq)\bmod M 
] 

và kiểm tra xem (1\le p\le10^9). Cặp ẩn được đảm bảo vượt qua bài kiểm tra này, vì vậy cặp hợp lệ đầu tiên sẽ đưa ra câu trả lời đúng. Điều này đơn giản về mặt khái niệm và hoàn toàn chính xác vì truy vấn cho chúng ta biết chính xác (rq\bmod M) phải bằng gì đối với mẫu số thực. 

Vấn đề là số lượng hoạt động. Trong trường hợp xấu nhất, một trường hợp thử nghiệm cần (10^9) phép nhân mô-đun. Trên (10^5) trường hợp thử nghiệm, điều này có thể đạt tới (10^{14}) lần lặp. Lực lượng vũ phu hoạt động vì mọi mẫu số có thể có thể được kiểm tra độc lập, nhưng nó không thành công vì phạm vi mẫu số quá lớn. 

Quan sát quan trọng là chúng ta được phép chọn các mô đun. Thay vì sử dụng mô đun chỉ lớn hơn (10^9), hãy chọn hai số nguyên tố cố định cực kỳ gần với (10^{12}): 

[ 
m_1=999999999989,\qquad 
m_2=999999999997. 
] 

Cả hai đều thỏa mãn hạn chế truy vấn và đều là số nguyên tố. 

Hai truy vấn là đủ. Theo Định lý phần dư Trung Hoa, hai phần dư có thể được kết hợp thành một phần dư (r) modulo 

[ 
M=m_1m_2. 
] 

Mô đun này xấp xỉ (10^{24}), do đó, cụ thể 

[ 
M>2\cdot10^{18}. 
] 

Giới hạn mạnh hơn đó cho phép chúng ta sử dụng phép tái cấu trúc hợp lý phân số tiếp tục tiêu chuẩn. 

Từ 

[ 
rq\equiv p\pmod M 
] 

tồn tại một số nguyên (k) sao cho 

[ 
rq-kM=p. 
] 

Chia cho (Mq): 

[ 
\frac rM-\frac{k}{q}=\frac{p}{Mq}. 
] 

Bởi vì (p,q\le10^9) và (M>2\cdot10^{18}), 

[ 
\left|\frac rM-\frac{k}{q}\right| 
=\frac{p}{Mq} 
<\frac{1}{2q^2}. 
] 

Một tính chất phân số tiếp tục cổ điển nói rằng một xấp xỉ hữu tỉ gần với (r/M) này phải là một trong những giá trị hội tụ của nó. Do đó, mẫu số ẩn (q) xuất hiện một cách tự nhiên trong quá trình thuật toán Euclide áp dụng cho (M) và (r). Kết nối phân số liên tục là một cách tiêu chuẩn để giải quyết vấn đề tái thiết hợp lý chính xác này. 

Điều này biến việc tìm kiếm (10^9) mẫu số có thể thành thuật toán Euclide (O(\log M)). Vì (M) chỉ có khoảng 24 chữ số thập phân nên chỉ có vài chục lần lặp Euclide cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(10^9)) mỗi trường hợp thử nghiệm | (O(1)) | Quá chậm | 
| CRT + phân số tiếp theo | (O(\log M)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Truy vấn số nguyên tố cố định đầu tiên (m_1=999999999989) và gọi phần dư được trả về (r_1). Truy vấn này hợp lệ vì (m_1) là số nguyên tố và nằm hoàn toàn giữa (10^9) và (10^{12}). 
2. Truy vấn số nguyên tố cố định thứ hai (m_2=999999999997), thu được (r_2). Hai mô đun là nguyên tố cùng nhau vì chúng là các số nguyên tố riêng biệt, đó chính là điều mà Định lý số dư Trung Hoa yêu cầu. 
3. Kết hợp hai đồng dư 

[ 
R\equiv r_1\pmod {m_1}, 
\qquad 
R\equiv r_2\pmod {m_2} 
] 

thành một dư lượng (R) modulo (M=m_1m_2). Viết 

[ 
R=r_1+m_1t. 
] 

Thay thế vào đồng đẳng thứ hai sẽ cho 

[ 
m_1t\equiv r_2-r_1\pmod {m_2}. 
] 

Vì (m_1) có modulo nghịch đảo (m_2), nên chúng ta có thể tính (t) với một nghịch đảo mô đun. 

1. Sau CRT, chúng ta biết 

[ 
R\equiv pq^{-1}\pmod M, 
] 

vậy 

[ 
Rq\equiv p\pmod M. 
] 

Do đó tồn tại số nguyên (k) thỏa mãn 

[ 
Rq-kM=p. 
] 

Mục tiêu bây giờ là khôi phục các số nguyên dương nhỏ (p,q) từ phương trình này. 

1. Xét số hữu tỷ (k/q). Từ phương trình trước, 

[ 
\frac RM-\frac{k}{q} 
=\frac{p}{Mq}. 
] 

Vì (p,q\le10^9) và (M>2\cdot10^{18}), vế phải hoàn toàn nhỏ hơn (1/(2q^2)). Do đó (k/q) là sự hội tụ của phân số liên tục của (R/M).

1. Chạy thuật toán Euclide trên (M) và (R), đồng thời theo dõi hệ số của (R) trong mọi phần còn lại. Khởi tạo 

[ 
M=0\cdot R+1\cdot M 
] 

và 

[ 
R=1\cdot R+0\cdot M. 
] 

Khi Euclid biến đổi hai số dư liên tiếp bằng cách sử dụng 

[ 
a\leftarrow b,\qquad 
b\leftarrow a-\lfloor a/b\rfloor b, 
] 

phép biến đổi tương tự có thể được áp dụng cho các hệ số (R) của chúng. Do đó mọi phần còn lại Euclide đều có dạng 

[ 
s=cR+dM. 
] 

1. Tìm (các) số dư có (1\le s\le10^9) và hệ số dương (c\le10^9). Đối với phần ẩn, mối quan hệ chính xác là 

[ 
p=qR-kM, 
] 

vì vậy tử số (p) của nó là phần dư Euclide và mẫu số (q) của nó là hệ số dương của (R). 

1. Xuất cặp đó. Nếu phân số ban đầu không được rút gọn, việc tái cấu trúc Euclide có thể trả về dạng rút gọn, đại diện cho cùng một số hữu tỷ và được chấp nhận. 

### Tại sao nó hoạt động 

Bất biến trung tâm là mọi phần dư Euclide được biểu diễn dưới dạng tổ hợp tuyến tính nguyên của (R) và (M), và hệ số được theo dõi của nó ghi lại số nhân của (R). Cặp ẩn thỏa mãn 

[ 
p=qR-kM, 
] 

nên (p) là một trong số dư có hệ số (q). Mô đun CRT lớn cho giới hạn gần đúng 

[ 
\left|\frac RM-\frac{k}{q}\right|<\frac1{2q^2}, 
] 

buộc (k/q) là một phân số hội tụ liên tục. Euclid liệt kê chính xác cấu trúc hội tụ cần thiết cho việc tái thiết. Vì (M>10^{18}), không thể có hai giá trị hữu tỉ khác nhau có cả tử số và mẫu số giới hạn bởi (10^9), nên ứng cử viên được thuật toán tìm thấy nhất thiết phải là số hữu tỷ ẩn. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

B = 10**9

# Both are primes and satisfy 10^9 < m < 10^12.
M1 = 999999999989
M2 = 999999999997

def crt(a1, m1, a2, m2):
    """
    Return x in [0, m1*m2) such that
        x == a1 (mod m1)
        x == a2 (mod m2)
    """
    t = ((a2 - a1) % m2) * pow(m1, -1, m2) % m2
    x = a1 + m1 * t
    return x, m1 * m2

def recover(r, mod):
    """
    Recover p, q with 1 <= p, q <= B from
        r == p * q^{-1} (mod mod).
    We have mod > 2 * B^2.
    """
    # r0 = s0 * r + ...
    # r1 = s1 * r + ...
    r0, r1 = mod, r
    s0, s1 = 0, 1

    while r1:
        # The first remainder is r itself, which matters for
        # integer answers such as 2/1.
        if 1 <= r1 <= B and 1 <= s1 <= B:
            if (r * s1) % mod == r1:
                return r1, s1

        q = r0 // r1

        r0, r1 = r1, r0 - q * r1
        s0, s1 = s1, s0 - q * s1

    raise RuntimeError("rational reconstruction failed")

def ask(m):
    print("?", m, flush=True)
    ans = int(input())
    if ans == -1:
        sys.exit(0)
    return ans

def main():
    t = int(input())

    for _ in range(t):
        r1 = ask(M1)
        r2 = ask(M2)

        r, mod = crt(r1, M1, r2, M2)
        p, q = recover(r, mod)

        print("!", p, q, flush=True)

if __name__ == "__main__":
    main()
```Hai hằng số được chọn có chủ ý gần với (10^{12}). Sản phẩm của họ là 

[ 
M=999999999986000000000033, 
] 

lớn hơn nhiều so với (2\cdot10^{18}). Việc sử dụng các mô đun lớn như vậy là điều cho phép chứng minh phân số liên tục sử dụng giới hạn gần đúng tiêu chuẩn (1/(2q^2)). 

các`ask`hàm in một truy vấn và ngay lập tức xóa thiết bị xuất chuẩn. Việc xóa là bắt buộc trong một bài toán tương tác vì nếu không, trọng tài có thể đợi mãi một truy vấn vẫn còn trong bộ đệm đầu ra của Python. Phản hồi của thẩm phán`-1`thông thường báo hiệu một tương tác không hợp lệ nên chương trình sẽ kết thúc ngay lập tức. 

các`crt`hàm sử dụng biểu diễn 

[ 
R=r_1+m_1t. 
] 

Giá trị của (t) được tìm thấy bằng cách sử dụng nghịch đảo mô đun của (m_1) modulo (m_2). Số nguyên Python có độ chính xác tùy ý, do đó mô đun CRT xấp xỉ (10^{24}) không gây ra tràn. Trong ngôn ngữ có chiều rộng cố định, sản phẩm này sẽ yêu cầu loại số nguyên rộng hơn số học 64-bit có dấu. 

các`recover`hàm là phiên bản mở rộng của thuật toán Euclide.`r1`là số dư hiện tại và`s1`là hệ số của nó so với dư lượng ban đầu (r). Điều bất biến là phần dư hiện tại có thể được viết là 

[ 
r_1=s_1r+cM 
] 

đối với một số nguyên (c). 

Việc kiểm tra ứng viên được thực hiện trước khi cập nhật Euclide. Ranh giới này là cần thiết cho các trường hợp như (2/1), trong đó bản thân phần dư đã là tử số mong muốn và mẫu số là (1). Bỏ qua bước kiểm tra này sẽ làm mất câu trả lời số nguyên. 

Việc kiểm tra mô-đun bổ sung```
(r * s1) % mod == r1
```không cần thiết cho chứng minh toán học, nhưng nó làm cho điều kiện ứng cử viên trở nên rõ ràng và bảo vệ việc thực hiện khỏi chấp nhận phần dư có hệ số không liên quan. 

Không có số học dấu phẩy động được sử dụng. Đặc biệt, cả (R/M) lẫn câu trả lời hợp lý đều không bao giờ được chuyển thành`float`, vì mô đun CRT nằm trong khoảng (10^{24}), vượt xa phạm vi mà biểu diễn dấu phẩy động có thể lưu giữ tất cả thông tin số nguyên có liên quan. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là bản ghi tương tác chứ không phải cặp đầu vào/đầu ra hàng loạt thông thường. Nó sử dụng mô đun đơn (1000000007), trong khi giải pháp trên có chủ ý sử dụng hai số nguyên tố lớn hơn để việc tái cấu trúc phân số tiếp tục có mô đun đủ mạnh. Mẫu của`1/2`trường hợp trả lại`500000004`và mẫu chấp nhận`2/4`; thay vào đó, thuật toán của chúng tôi sẽ trả về biểu diễn rút gọn`1/2`. 

Đối với ví dụ hoạt động đầu tiên, hãy lấy giá trị của mẫu (x=1/2). Với 

[ 
m_1=999999999989 
] 

nghịch đảo của (2) là (500000000000), do đó thẩm phán trả về (500000000000). Với 

[ 
m_2=999999999997, 
] 

nghịch đảo của (2) là (499999999999). CRT kết hợp những điều này thành 

[ 
R=499999999993000000000017. 
] 

Mô đun kết hợp là 

[ 
M=999999999986000000000033. 
] 

Vì (M=2R-1), các bước Euclide đầu tiên đặc biệt đơn giản. 

| Trạng thái Euclide | Phần còn lại hiện tại | Hệ số (R) | Ứng viên | 
| --- | --- | --- | --- | 
| Ban đầu | 4999999999930000000000017 | 1 | Quá lớn | 
| Sau (MR) | 499999999992999999999? | -1 | Hệ số âm | 
| Sau khi phân chia tiếp theo | 1 | 2 | (p=1,q=2) | 

Phần còn lại ở giữa chính xác là (R-1=499999999993000000000016). Phần dư nhỏ cuối cùng thỏa mãn 

[ 
1=2R-M, 
] 

nên hệ số của (R) là (2), cho ra (p=1,q=2). Điều này chứng tỏ tại sao việc theo dõi hệ số lại hữu ích: thuật toán Euclide đồng thời tìm ra tử số nhỏ và mẫu số tương ứng của nó. 

Đối với ví dụ hoạt động thứ hai, lấy giá trị nguyên của mẫu (x=2/1). Thẩm phán trả về (2) cho cả hai số nguyên tố lớn, do đó CRT ngay lập tức đưa ra 

[ 
R=2. 
] 

Việc tái thiết Euclide bắt đầu với chính phần dư. 

| Trạng thái Euclide | Phần còn lại hiện tại | Hệ số (R) | Ứng viên | 
| --- | --- | --- | --- | 
| Ban đầu | 2 | 1 | (p=2,q=1) | 
| Tiếp theo | 1 | Hệ số âm lớn | Bị từ chối | 
| Cuối cùng | 0 | Không được xem xét | Dừng lại | 

Trạng thái đầu tiên đã là một câu trả lời hợp lệ. Đây là trường hợp đặc biệt thúc đẩy việc kiểm tra số dư ban đầu trước khi thực hiện phép chia Euclide đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(t\log M)) | Hai truy vấn, CRT và một thuật toán Euclide cho mỗi trường hợp thử nghiệm | 
| Không gian | (O(1)) | Chỉ một số nguyên không đổi được lưu trữ | 

Ở đây (M) xấp xỉ (10^{24}), do đó thuật toán Euclide chỉ mất (O(\log M)), khoảng vài chục lần lặp. Đối với (10^5) trường hợp thử nghiệm, điều này có thể dễ dàng quản lý được về mặt công việc số học. Bản thân giao thức tương tác chiếm ưu thế về chi phí thực tế vì mọi trường hợp thử nghiệm đều yêu cầu hai lần trao đổi phản hồi truy vấn, vẫn nằm trong số mười truy vấn được phép cho mỗi trường hợp. 

## Trường hợp thử nghiệm 

Vì đây là sự cố tương tác nên mẫu được cung cấp không thể được chuyển vào chương trình dưới dạng đầu vào hàng loạt thông thường. Đầu vào mẫu bao gồm các câu trả lời của thẩm phán, trong khi chương trình dự kiến ​​​​sẽ in các truy vấn trước khi nhận được những câu trả lời đó. Thay vào đó, khai thác thử nghiệm sau đây sẽ kiểm tra việc tái cấu trúc toán học một cách độc lập bằng cách mô phỏng đánh giá cho một số phân số ẩn.```python
import io
import sys

B = 10**9
M1 = 999999999989
M2 = 999999999997

def crt(a1, m1, a2, m2):
    t = ((a2 - a1) % m2) * pow(m1, -1, m2) % m2
    return a1 + m1 * t, m1 * m2

def recover(r, mod):
    r0, r1 = mod, r
    s0, s1 = 0, 1

    while r1:
        if 1 <= r1 <= B and 1 <= s1 <= B:
            if (r * s1) % mod == r1:
                return r1, s1

        q = r0 // r1
        r0, r1 = r1, r0 - q * r1
        s0, s1 = s1, s0 - q * s1

    raise AssertionError("reconstruction failed")

def residue(p, q, mod):
    return p * pow(q, -1, mod) % mod

def solve_hidden(p, q):
    r1 = residue(p, q, M1)
    r2 = residue(p, q, M2)

    r, mod = crt(r1, M1, r2, M2)
    return recover(r, mod)

# Sample-derived cases.
assert solve_hidden(1, 1) == (1, 1), "sample value 1/1"
assert solve_hidden(1, 2) == (1, 2), "sample value 1/2"
assert solve_hidden(2, 1) == (2, 1), "sample value 2/1"

# Minimum numerator and denominator.
assert solve_hidden(1, 1) == (1, 1), "minimum values"

# Maximum values, with a reducible fraction.
assert solve_hidden(10**9, 10**9) == (1, 1), "maximum equal values"

# Both numerator and denominator are at the upper boundary,
# but the fraction is not reducible.
assert solve_hidden(10**9, 999999999) == (1000000000, 999999999), \
    "maximum boundary fraction"

# A fraction just below the upper numerator boundary.
assert solve_hidden(999999999, 1000000000) == (999999999, 1000000000), \
    "boundary numerator and denominator"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (1/1) | (1/1) | Giá trị tối thiểu và dư lượng không đổi | 
| (1/2) | (1/2) | Mẫu hữu tỉ không nguyên | 
| (2/1) | (2/1) | Phần dư Euclide ban đầu | 
| (10^9/10^9) | (1/1) | Phân số có kích thước tối đa có thể giảm | 
| (10^9/999999999) | (10^9/999999999) | Cả hai giá trị gần ranh giới trên | 
| (999999999/10^9) | (999999999/10^9) | Mẫu số và tử số biên | 

Việc kiểm tra (10^9/10^9) đặc biệt hữu ích vì cặp ban đầu không nguyên tố cùng nhau. Việc xây dựng lại trả về (1/1), có cùng giá trị hợp lý và là một câu trả lời hợp pháp. Hai kiểm tra ranh giới cũng xác nhận rằng việc triển khai không bao giờ dựa vào bất đẳng thức nghiêm ngặt như (p<10^9) hoặc (q<10^9). 

## Vỏ cạnh 

Đối với phân số không rút gọn (2/4), giá trị ẩn là (1/2). Đối với số nguyên tố lớn đầu tiên, số dư là (5000000000000) và đối với số nguyên tố thứ hai là (499999999999). CRT tạo ra phần dư kết hợp giống như đối với (1/2), bởi vì phép chia theo mô-đun chỉ phụ thuộc vào giá trị hữu tỷ, chứ không phụ thuộc vào biểu diễn tử số và mẫu số nào được sử dụng. Việc xây dựng lại trả về (1/2), được chấp nhận mặc dù bản ghi mẫu sử dụng (2/4). 

Đối với số nguyên (2/1), cả hai phần dư được truy vấn đều chính xác là (2), do đó CRT trả về (R=2). Việc xây dựng lại kiểm tra trạng thái Euclide ban đầu (R=2) với hệ số (1), ngay lập tức thu được (p=2,q=1). Đây là lý do tại sao việc kiểm tra ứng viên phải diễn ra trước phép chia Euclide đầu tiên. 

Đối với (1/1), mọi truy vấn đều trả về (1) và CRT cũng trả về (R=1). Trạng thái ban đầu có phần dư (1) và hệ số (1), do đó thuật toán đưa ra (1/1) ngay lập tức. Không có nhánh trường hợp đặc biệt nào cho số (1) được yêu cầu. 

Đối với cặp bằng nhau tối đa (10^9/10^9), giá trị mô-đun vẫn chính xác là (1), vì phân số giảm xuống (1/1). Thuật toán trả về (1/1), chứng minh tại sao đầu ra phải được hiểu là một giá trị hợp lý chứ không phải là cặp ẩn chính xác. 

Đối với một phân số biên, chẳng hạn như (1000000000/999999999), cả tử số và mẫu số đều nằm trong phạm vi cho phép nhưng tích của chúng gần bằng (10^{18}). Mô-đun CRT nằm trong khoảng (10^{24}), để lại giới hạn an toàn rất lớn. Thuật toán Euclide hoạt động hoàn toàn với các số nguyên và xây dựng lại phân số mà không làm mất đi độ chính xác. 

Lỗi triển khai nguy hiểm nhất là chỉ sử dụng mô đun xung quanh (10^9) và sau đó giả sử các phân số tiếp tục tự động đủ. Giới hạn hội tụ tiêu chuẩn yêu cầu (M>2B^2), trong khi mô đun truy vấn riêng lẻ của vấn đề ban đầu nhỏ hơn nhiều. Hai số nguyên tố lớn giải quyết chính xác vấn đề này: tích của chúng lớn hơn nhiều so với (2B^2), trong khi mỗi truy vấn riêng lẻ vẫn nằm trong phạm vi cho phép. Đây là lý do chính khiến thuật toán cuối cùng vừa ngắn vừa nghiêm ngặt.
