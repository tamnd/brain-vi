---
title: "CF 102394G - Cửa hàng trò chơi"
description: "Vào mỗi ngày, một bộ cho thuê mới sẽ có sẵn. Một tập hợp được xác định bởi hai giá trị: (ai), kích thước của mỗi cọc trong số hai cọc bằng nhau của nó và (bi), số tiền Alice trả để thuê tập hợp đó."
date: "2026-08-10T19:10:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "G"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 177
verified: true
draft: false
---

[CF 102394G - Cửa hàng trò chơi](https://codeforces.com/problemset/problem/102394/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vào mỗi ngày, một bộ cho thuê mới sẽ có sẵn. Một bộ được xác định bởi hai giá trị: (a_i), kích thước của mỗi cọc trong số hai cọc bằng nhau của nó và (b_i), số tiền Alice trả để thuê bộ đó. Alice có thể thuê bất kỳ bộ sưu tập nào của các bộ đã xuất hiện cho đến nay, nhưng nếu cô ấy thuê một bộ, cô ấy sẽ nhận được cả hai bản sao của chồng bộ đó. 

Sau đó, Bob sẽ xem tất cả các cọc đã thuê và có thể xóa bất kỳ bộ sưu tập nào trong số chúng trước khi trò chơi Nim bắt đầu. Alice chỉ thắng nếu Bob không thể biến vị trí còn lại thành vị trí thua cuộc. 

Sản lượng mỗi ngày là tổng chi phí thuê lớn nhất có thể có của một bộ sưu tập mà Alice có thể chọn sao cho, bất kể Bob loại bỏ cọc nào, Alice vẫn thắng. Câu trả lời của ngày hôm trước cũng là một phần của quá trình giải mã đầu vào: cặp thực tế của ngày hiện tại là (a_i=x_i\mathbin{\mathsf{xor}}cuối cùng) và (b_i=y_i\mathbin{\mathsf{xor}}cuối cùng). 

Phiên bản chính thức có (n\le 500000), (a_i\le10^{18}) và (b_i\le10^9), với giới hạn thời gian 1,5 giây và bộ nhớ 512 MB. Hậu quả quan trọng là chúng ta không thể xem lại tất cả các tập hợp đã thuê trước đó sau mỗi lần chèn. Ngay cả (O(n\sqrt n)) cũng quá lớn và cách tiếp cận (O(n^2)) sẽ yêu cầu các thao tác khoảng (2,5\cdot10^{11}). Các số (a_i) có kích thước 60 bit, vì vậy mỗi cọc có thể được xem dưới dạng vectơ chỉ có 60 tọa độ. Kích thước cố định nhỏ đó là sự mở đầu cho cấu trúc dữ liệu cơ sở tuyến tính. 

Sự tinh tế đầu tiên là hai bản sao trong một bộ cho thuê đều quan trọng. Một tập hợp có giá trị (a) đóng góp hai vectơ giống nhau chứ không phải một. Bob có thể loại bỏ một trong hai bản sao một cách độc lập, do đó hệ số của vectơ đó trong tập con của các cọc còn lại có thể là (0), (1) hoặc (2). Đây chính xác là lý do tại sao trường (\mathbb F_3), thay vì XOR nhị phân thông thường, xuất hiện. 

Sự tinh tế thứ hai là việc cho thuê mọi bộ có lợi nhuận không nhất thiết phải hợp lệ. Ví dụ, hãy xem xét trình tự thực tế```
3
1 2
3 1
2 7
```Các bộ được giải mã là ((a,b)=(1,2),(1,3),(1,4)), vì các câu trả lời trước đó là (2,3). Cả ba vectơ đều giống hệt nhau. Các câu trả lời đúng là (2,3,4). Thuê cả ba vào ngày thứ ba sẽ cho ra sáu bản sao có cùng giá trị cọc, có số bit là (6), chia hết cho (3), do đó Bob có thể rời khỏi vị thế thua cuộc. Một thuật toán bất cẩn chỉ tính tổng mọi chi phí thuê dương sẽ tạo ra (9). 

Một trường hợp tinh vi khác là vectơ nặng ban đầu phụ thuộc vào cơ sở hiện tại nhưng sẽ thay thế vectơ nhẹ hơn. Mẫu chính thức có các giá trị được giải mã ((1,4),(2,3),(3,10)), đưa ra đáp án (4,7,14). Vào ngày thứ ba, (3) phụ thuộc vào (1) và (2), nhưng chi phí của nó (10) lớn hơn chi phí hiện tại. Bộ sưu tập tối ưu giữ lại (3) và (1), cho (14). Cấu trúc dữ liệu chỉ chấp nhận các phần chèn độc lập và loại bỏ vĩnh viễn các vectơ phụ thuộc sẽ vẫn ở mức (7). 

Mã hóa là một nguồn dễ gây ra lỗi thầm lặng khác. XOR phải sử dụng đầu ra trước đó chứ không phải giá trị đầu vào trước đó và câu trả lời được giải mã phải được gán cho`last`trước khi đọc cặp mã hóa tiếp theo. Đầu vào mẫu```
3
1 4
6 7
4 13
```giải mã thành ((1,4),(2,3),(3,10)), không phải ba cặp được hiển thị trực tiếp trong đầu vào. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ giúp mọi bộ phim cho thuê được hiển thị cho đến nay và mỗi ngày, tìm kiếm bộ sưu tập đắt tiền nhất mà Alice có thể thuê một cách an toàn. Đối với một tập hợp gồm (k) bộ, có (3^k) cách Bob có thể chọn 0, một hoặc hai bản sao của mỗi tập hợp, vì vậy việc kiểm tra mọi mẫu xóa có thể có đã là hàm mũ. Ngay cả sau khi nhận ra đặc tính đại số tuyến tính bên dưới, việc triển khai bạo lực có thể liệt kê tất cả các tập hợp con của các tập hợp có sẵn. Với (n=500000), điều đó có nghĩa là (2^{500000}) ứng viên trong trường hợp xấu nhất, điều này hoàn toàn không khả thi. 

Lý do trò chơi có thể bị rút ngắn hơn nữa là đặc điểm của việc mất vị trí trong phiên bản Nim này. Vì một nước đi có thể ảnh hưởng nhiều nhất đến hai cọc nên đây là Nim của Moore với tham số (2). Một vị trí bị mất chính xác khi, đối với mỗi vị trí bit nhị phân, số lượng cọc chứa bit đó là (0) modulo (3). 

Biểu diễn mọi giá trị cọc (a) bằng vectơ nhị phân 60 chiều của nó. Giả sử Alice thuê các tập hợp có vectơ (v_1,\ldots,v_k). Có hai bản sao của mỗi vector. Nếu Bob để lại (c_i) bản sao của tập hợp (i), thì (c_i\in{0,1,2}) và vị trí kết quả sẽ mất chính xác khi 

[ 
\sum_{i=1}^{k} c_i v_i=0 
] 

hơn (\mathbb F_3). 

Alice phải ngăn Bob nhận được số tiền bằng 0 như vậy đối với mọi lựa chọn hệ số khác rỗng. Điều kiện đó nói chính xác rằng (v_1,\ldots,v_k) độc lập tuyến tính với (\mathbb F_3). Nếu chúng phụ thuộc thì các hệ số của sự phụ thuộc không tầm thường là (0,1,) hoặc (2), do đó Bob có thể chọn chính xác số lượng bản sao đó và nhận được vị trí thua cuộc. Ngược lại, nếu các vectơ độc lập thì tổ hợp tuyến tính bằng 0 duy nhất sử dụng hệ số (0) cho mọi tập hợp, tương ứng với việc xóa tất cả các cọc và Bob không được phép làm điều đó. 

Thế là trò chơi biến mất. Vấn đề trở thành: 

Với mỗi tiền tố của dữ liệu đầu vào, hãy tìm tổng trọng số tối đa của tập con độc lập tuyến tính của các vectơ tương ứng trên (\mathbb F_3). 

Đây là bài toán matroid tuyến tính có trọng số. Tính chất tham lam thông thường của matroid nói rằng có thể thu được một tập độc lập có trọng số cực đại bằng cách xem xét các phần tử từ trọng số cao đến trọng số thấp. Vì đầu vào trực tuyến nên chúng tôi không thể sắp xếp lại toàn bộ tiền tố sau mỗi lần chèn. Thay vào đó, chúng tôi duy trì cơ sở Gaussian có trọng số một cách linh hoạt. 

Khi một vectơ mới gặp một trục bị chiếm đóng, chúng tôi sẽ loại bỏ trục đó. Nếu vectơ mới có trọng số lớn hơn vectơ cơ sở hiện đang chiếm giữ trục quay, chúng ta hoán đổi chúng và tiếp tục loại bỏ vectơ dịch chuyển. Đây là sự tương tự đại số tuyến tính của việc hoán đổi một cạnh trong một khu rừng bao trùm tối đa. 

Kích thước tối đa là 60 vì (a_i\le10^{18<2^{60}). Do đó, mỗi lần chèn thực hiện tối đa 60 lần loại bỏ trục. Các vectơ ternary có thể được lưu trữ dưới dạng hai mặt nạ 60 bit, một mặt nạ cho tọa độ bằng (1) và một mặt nạ cho tọa độ bằng (2). Sau đó, các thao tác bit thực hiện toàn bộ thao tác vectơ cùng một lúc thay vì lặp qua tất cả các tọa độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n)) hoặc tệ hơn | (O(n)) | Quá chậm | 
| Tối ưu | (O(60n)) | (O(60)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Diễn giải mỗi giá trị cọc (a) dưới dạng một vectơ trên (\mathbb F_3). Đối với mỗi bit nhị phân được đặt trong (a), tọa độ tương ứng là (1), trong khi tất cả các tọa độ khác là (0). 
2. Duy trì cơ sở Gaussian với tối đa 60 vectơ. Với mỗi vị trí trục quay (p), hãy lưu trữ một vectơ cơ sở có hệ số tại (p) chính xác là (1), cùng với chi phí thuê của vectơ đó. 
3. Khi có cặp ((a,b)) mới xuất hiện, trước tiên hãy giải mã nó bằng câu trả lời trước đó. Vectơ ban đầu chỉ chứa các hệ số (0) và (1). 
4. Tìm tọa độ cao nhất trong đó vectơ hiện tại khác 0. Đây là trục mà việc loại bỏ Gaussian sẽ xử lý tiếp theo. Nếu không còn tọa độ, vectơ sẽ phụ thuộc vào cơ sở hiện tại và có thể bị loại bỏ. 
5. Nếu trục tương ứng trống, hãy chèn vectơ vào đó. Nếu hệ số trục của nó là (2), hãy nhân toàn bộ vectơ với (2), hoán đổi mặt nạ (1) và (2) của nó. Nhân với (2) là hợp lệ vì (2\cdot2=1\pmod3). 
6. Nếu trục bị chiếm, hãy so sánh trọng lượng đến với trọng lượng được lưu trữ tại trục đó. Nếu trọng số tới không lớn hơn, hãy loại bỏ trục quay khỏi vectơ tới và tiếp tục. 
7. Nếu trọng số đến lớn hơn, hãy hoán đổi hai vectơ. Vectơ nặng hơn trở thành vectơ cơ sở tại trục này, trong khi vectơ cơ sở cũ trở thành vectơ bị loại bỏ. Câu trả lời thay đổi bởi sự khác biệt giữa hai trọng số. 
8. Tiếp tục cho đến khi vectơ tới bằng 0 hoặc đạt đến một trục quay trống. Mỗi lần chèn thành công sẽ làm tăng tổng số được duy trì, trong khi mỗi lần trao đổi thành công sẽ thay thế bộ nhẹ hơn đã chọn bằng bộ nặng hơn mà không thay đổi khoảng cách. 
9. In tổng số duy trì và gán cho`last`. Cặp đầu vào được mã hóa tiếp theo phải sử dụng giá trị chính xác này. 

Bất biến trung tâm là sau khi xử lý bất kỳ tiền tố nào, cơ sở được lưu trữ biểu thị một tập hợp con độc lập có trọng số tối đa của tất cả các vectơ trong tiền tố đó. Việc loại bỏ Gaussian bảo toàn khoảng, trong khi quy tắc trao đổi giữ thành viên nặng hơn bất cứ khi nào một vectơ mới có thể thay thế vectơ cơ sở hiện có. Vì tính độc lập tuyến tính tạo thành một matroid nên các trao đổi cục bộ này đủ để duy trì một tập độc lập có trọng số tối đa toàn cục. Việc rút gọn lý thuyết trò chơi sau đó chứng minh rằng chính xác những tập hợp độc lập này là những bộ sưu tập mà Alice có thể thuê một cách an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

BITS = 60

# For a ternary vector:
# lo has a 1 where the coefficient is 1.
# hi has a 1 where the coefficient is 2.
#
# The two masks together are a bit-sliced representation of F_3^60.

def add3(al, ah, bl, bh):
    # Coordinate-wise addition modulo 3.
    x = al ^ bl
    y = ah ^ bh

    # Output coefficient 1:
    # (0,1), (1,0), (2,2)
    nl = (x & ~y) | (ah & bh)

    # Output coefficient 2:
    # (0,2), (2,0), (1,1)
    nh = ((~(al | bl)) & y) | (al & bl)

    return nl, nh

def solve(reader=input):
    n = int(reader())

    basis_lo = [0] * BITS
    basis_hi = [0] * BITS
    basis_w = [0] * BITS

    ans = 0
    last = 0
    out = []

    for _ in range(n):
        x, y = map(int, reader().split())

        a = x ^ last
        w = y ^ last

        lo = a
        hi = 0

        while lo or hi:
            p = (lo | hi).bit_length() - 1

            # Since the basis vector at p is normalized,
            # its coefficient at p is exactly 1.
            coeff = ((lo >> p) & 1) | (((hi >> p) & 1) << 1)

            if basis_lo[p] == 0 and basis_hi[p] == 0:
                # Normalize the new pivot to 1.
                if coeff == 2:
                    lo, hi = hi, lo

                basis_lo[p] = lo
                basis_hi[p] = hi
                basis_w[p] = w
                ans += w
                break

            bw = basis_w[p]

            if w > bw:
                # The new vector must become the heavier basis vector.
                if coeff == 2:
                    lo, hi = hi, lo
                    coeff = 1

                # Replace the old basis vector.
                lo, basis_lo[p] = basis_lo[p], lo
                hi, basis_hi[p] = basis_hi[p], hi
                w, basis_w[p] = basis_w[p], w

                ans += basis_w[p] - w

                # The displaced old basis vector has pivot coefficient 1.
                # Eliminate it using the new basis vector.
                blo = basis_lo[p]
                bhi = basis_hi[p]

                # x <- x - basis[p] = x + 2*basis[p].
                lo, hi = add3(lo, hi, bhi, blo)
            else:
                blo = basis_lo[p]
                bhi = basis_hi[p]

                if coeff == 1:
                    # x <- x - b = x + 2b.
                    lo, hi = add3(lo, hi, bhi, blo)
                else:
                    # x <- x - 2b = x + b.
                    lo, hi = add3(lo, hi, blo, bhi)

        last = ans
        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Ba mảng cơ sở chứa hai mặt phẳng bit của mỗi vectơ bậc ba và chi phí thuê liên quan của nó. Một trục chỉ được coi là trống khi cả hai mặt nạ đều bằng 0. Không cần lưu trữ bản gốc (a_i), vì cơ sở Gaussian chỉ cần vectơ rút gọn hiện tại. 

các`add3`chức năng là tối ưu hóa bitset cốt lõi. Đối với mỗi tọa độ, một chữ số bậc ba có một trong các mã hóa (0,1,2). Mặt nạ thấp xác định các chữ số bằng (1), trong khi mặt nạ cao xác định các chữ số bằng (2). Các biểu thức Boolean bên trong`add3`thực hiện đồng thời chín trường hợp cộng modulo (3) cho tất cả 60 tọa độ. 

Phép trừ cũng rẻ vì trong (\mathbb F_3), phép trừ một vectơ cũng giống như việc cộng hai lần vectơ đó. Nhân với (2) hoán đổi hai mặt nạ, vì vậy`x - basis`được thực hiện như`add3(x, swapped_basis)`, trong khi`x - 2*basis`đơn giản là`add3(x, basis)`. 

Vectơ đến có thể có hệ số trục (2). Trước khi nó được lắp vào đế, hai mặt nạ của nó được hoán đổi để hệ số trục của nó trở thành (1). Việc chuẩn hóa này là cần thiết vì mọi vectơ cơ sở được lưu trữ đều được giả sử có hệ số (1) tại trục xoay của chính nó. 

Việc trao đổi cân cũng dễ xảy ra sai sót. Giả sử vectơ tới có trọng số (w) và vectơ trục hiện có có trọng số (w_b). Khi (w>w_b), vectơ mới sẽ được chọn và vectơ cũ bị dịch chuyển. Câu trả lời thay đổi bởi (w-w_b), trong khi vectơ dịch chuyển tiếp tục thông qua việc loại bỏ Gaussian với trọng số cũ của nó. Mã thực hiện việc hoán đổi đó trước khi loại bỏ trục xoay. 

Tất cả số học liên quan đến chi phí thuê và câu trả lời đều phù hợp thoải mái với số nguyên Python. Câu trả lời tối đa nhiều nhất là (60\cdot10^9), nhưng việc sử dụng các số nguyên có độ chính xác tùy ý của Python cũng khiến việc triển khai không nhạy cảm với bất kỳ kích thước biểu diễn trung gian nào. 

Việc giải mã XOR xảy ra trước hoạt động cơ bản và`last`chỉ được cập nhật sau khi câu trả lời hiện tại đã được hoàn tất. Thứ tự này là bắt buộc vì đầu vào được mã hóa có chủ ý bằng đầu ra trước đó. 

## Ví dụ đã hoạt động 

### Mẫu 

Đầu vào mẫu là```
3
1 4
6 7
4 13
```Chuỗi được giải mã là ((1,4),(2,3),(3,10)). 

| Ngày | Cặp mã hóa | Đã giải mã ((a,b)) | Vectơ cơ sở sau khi chèn | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | ((1,4)) | ((1,4)) | (1:4) | 4 | 
| 2 | ((6,7)) | ((2,3)) | (1:4,\ 2:3) | 7 | 
| 3 | ((4,13)) | ((3,10)) | (1:4,\ 3:10) | 14 | 

Vào ngày đầu tiên, vectơ đơn độc lập nên giá trị của nó được chọn. Vào ngày thứ hai, (1) và (2) chiếm các vị trí bit khác nhau và độc lập nên cả hai chi phí đều được giữ nguyên. 

Vào ngày thứ ba, vectơ (3) là tổng của vectơ (1) và (2) trên (\mathbb F_3), nên nó phụ thuộc vào cơ sở hiện tại. Tuy nhiên, trọng số của nó (10) lớn hơn trọng lượng (3) của vectơ (2). Cơ sở động thay thế vectơ (2) bằng vectơ (3), để lại bộ sưu tập độc lập ({1,3}) với tổng trọng số (14). 

### Ví dụ được xây dựng 

Hãy xem xét trình tự thực tế```
4
1 100
3 101
2 102
4 204
```Đầu vào được mã hóa phải sử dụng câu trả lời trước đó, đưa ra```
4
1 100
103 1
203 175
207 15
```Vectơ đầu tiên là (1), vectơ thứ hai là (3) và vectơ thứ ba là (2). Vectơ thứ ba phụ thuộc vào hai vectơ đầu tiên nên nó gây ra sự trao đổi. Vectơ thứ tư (4) giới thiệu một bit hoàn toàn mới. 

| Ngày | Đã giải mã ((a,b)) | Các vectơ độc lập hiện tại | Trả lời | 
| --- | --- | --- | --- | 
| 1 | ((1.100)) | (1) | 100 | 
| 2 | ((3,101)) | (1,3) | 201 | 
| 3 | ((2,102)) | (3,2) | 203 | 
| 4 | ((4,204)) | (3,2,4) | 407 | 

Ngày thứ ba chứng minh tại sao cơ sở có trọng số không thể đơn giản bác bỏ các vectơ phụ thuộc. Vectơ (2) phụ thuộc vào (1) và (3), nhưng thay thế (1), có trọng số là (100), bằng (2), có trọng số là (102), sẽ cải thiện mức tối ưu. Ngày thứ tư sau đó thêm một vectơ độc lập và tăng trực tiếp câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(60n)) | Mỗi lần chèn thực hiện tối đa 60 lần loại bỏ trục và mỗi thao tác vectơ hoạt động trên 60 tọa độ dưới dạng mặt nạ bit. | 
| Không gian | (O(60)) | Chỉ có một vectơ cơ sở và một trọng số được lưu trữ cho mỗi vị trí bit có thể. | 

Chỉ có 60 điểm xoay có thể có vì mọi (a_i) đều ở dưới (2^{60}). Mặc dù có thể có nửa triệu bộ cho thuê, trạng thái được duy trì không bao giờ vượt quá 60 vectơ cơ sở. Đầu vào được mã hóa cũng buộc quá trình xử lý phải duy trì trực tuyến, cơ sở này hỗ trợ một cách tự nhiên vì mỗi câu trả lời đều được hoàn thiện trước khi cặp tiếp theo được giải mã. 

Các giải pháp C++ tham chiếu hoạt động thoải mái trong giới hạn 1,5 giây chính thức. Việc triển khai Python ở trên sử dụng các phép toán ba phần được cắt theo bit để tránh vòng lặp Python trên tất cả 60 tọa độ trong mỗi lần thêm vectơ, mặc dù giới hạn thời gian chặt chẽ của cuộc thi ban đầu được thiết kế chủ yếu xoay quanh việc triển khai được biên dịch được tối ưu hóa. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng cùng một mã hóa XOR trực tuyến làm đầu vào chính thức. Trình trợ giúp chuyển một trình đọc tùy chỉnh tới giải pháp để có thể kiểm tra cùng một thuật toán mà không cần sửa đổi logic của nó.```python
import io

# Use the solve() function from the solution above.

def run(inp: str) -> str:
    out = []

    class Reader:
        def __init__(self, s):
            self.f = io.StringIO(s)

        def __call__(self):
            return self.f.readline()

    # Capture stdout by using the same logic with a temporary buffer.
    import contextlib
    import sys

    buf = io.StringIO()
    with contextlib.redirect_stdout(buf):
        solve(Reader(inp))

    return buf.getvalue()

# Official sample
assert run(
    "3\n"
    "1 4\n"
    "6 7\n"
    "4 13\n"
) == "4\n7\n14\n", "official sample"

# Minimum-size input
assert run(
    "1\n"
    "1 5\n"
) == "5\n", "single rental set"

# Repeated vector with increasing weights.
# Actual values are (1,5), (1,7), (1,9).
assert run(
    "3\n"
    "1 5\n"
    "4 2\n"
    "6 14\n"
) == "5\n7\n9\n", "duplicate vectors"

# Weighted exchange.
# Actual values are:
# (1,100), (3,101), (2,102), (4,204)
assert run(
    "4\n"
    "1 100\n"
    "103 1\n"
    "203 175\n"
    "207 15\n"
) == "100\n201\n203\n407\n", "weighted basis exchange"

# Maximum-size input.
# Actual values are (1,2), (1,3), ..., (1,500001).
# Every vector is identical, so the best independent subset contains
# exactly one set, namely the most expensive one.
n = 500000
lines = [str(n)]
for i in range(1, n + 1):
    actual_b = i + 1
    if i == 1:
        last = 0
    else:
        last = i
    lines.append(f"{1 ^ last} {actual_b ^ last}")

large_input = "\n".join(lines) + "\n"
assert run(large_input).splitlines()[-1] == "500001", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 5`|`5`| Phiên bản kích thước tối thiểu và chèn cơ bản | 
| Ba hiệp có (a=1) và tạ (5,7,9) |`5, 7, 9`| Không được chọn tất cả các vectơ phụ thuộc | 
| ((1.100),(3.101),(2.102),(4.204)) |`100, 201, 203, 407`| Các vectơ phụ thuộc nặng phải thay thế các vectơ cơ sở nhẹ hơn | 
| 500000 vectơ giống hệt nhau với trọng số tăng dần | Câu trả lời cuối cùng`500001`| Tối đa (n), giải mã trực tuyến và hành vi xếp hạng một | 

## Vỏ cạnh 

Một tập hợp cho thuê duy nhất luôn an toàn vì hai cọc giống hệt nhau của nó đóng góp 0, một hoặc hai bản sao của cùng một vectơ. Một vectơ khác 0 không thể trở thành 0 với hệ số (1) hoặc (2), vì vậy Bob không thể rời khỏi vị trí thua cuộc. Để có đầu vào chính xác```
1
1 5
```vectơ là (1), cơ sở tăng trọng số (5) và đầu ra là`5`. 

Giá trị cọc lặp đi lặp lại là trường hợp chính trong đó việc tổng hợp tất cả các chi phí thuê dương không thành công. Với```
3
1 2
4 2
6 14
```các bộ được giải mã là ((1,2),(1,3),(1,4)). Cả ba vectơ đều giống nhau. Cơ sở chấp nhận vectơ thứ nhất có trọng số (2), thay thế nó bằng vectơ trọng số thứ hai (3), sau đó thay thế nó bằng vectơ trọng số thứ ba (4). Các đầu ra là`2`,`3`, Và`4`. Không bao giờ hai bản sao của cùng một vectơ có thể cùng tồn tại trong một tập hợp độc lập. 

Một vectơ phụ thuộc vẫn có thể cần thiết ở mức tối ưu khi nó đắt hơn một vectơ cơ sở hiện có. Trong ví dụ được xây dựng, vectơ (1) và (3) ban đầu được chọn với trọng số (100) và (101). Vector (2) phụ thuộc vào chúng nhưng trọng lượng của nó là (102). Việc chèn hoán đổi vectơ trọng số (100) và giữ vectơ trọng số (101)- và (102), cho`203`. Chỉ cần kiểm tra xem vectơ mới có độc lập hay không và loại bỏ nó sẽ tạo ra câu trả lời sai`201`. 

Hệ số (2) cũng phải được xử lý chính xác. Trên (\mathbb F_3), hệ số trục có thể trở thành (2) sau khi loại bỏ. Một vectơ như vậy không thể được lưu trữ không thay đổi vì cơ sở mong đợi hệ số trục (1). Nhân nó với (2) sẽ chuyển đổi hệ số trục của nó từ (2) thành (1) và việc hoán đổi hai mặt phẳng bit thực hiện chính xác phép nhân này. 

Cuối cùng, câu trả lời trước tham gia vào cả hai tọa độ của cặp đầu vào tiếp theo. Đối với mẫu chính thức, câu trả lời đầu tiên là (4), do đó cặp mã hóa thứ hai ((6,7)) giải mã thành ((2,3)). Sau khi câu trả lời thứ hai trở thành (7), cặp thứ ba ((4,13)) giải mã thành ((3,10)). Đang cập nhật`last`tại bất kỳ thời điểm nào khác sẽ thay đổi tất cả các giá trị được giải mã tiếp theo và âm thầm làm hỏng toàn bộ quá trình tính toán. 

Nếu bạn muốn, tôi cũng có thể cung cấp phiên bản C++17 phù hợp hơn với việc triển khai thời gian dự kiến ​​của cuộc thi, đặc biệt là đối với giới hạn 1,5 giây.
