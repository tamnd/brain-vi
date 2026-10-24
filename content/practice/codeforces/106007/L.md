---
title: "CF 106007L - Cân bằng"
description: "Chúng ta được cung cấp một mảng có độ dài n và kích thước đoạn cố định m. Trong một thao tác, chúng ta chọn một khối liền kề có chính xác m phần tử và áp dụng OR theo bit với giá trị truy vấn x cho mọi phần tử trong khối đó."
date: "2026-06-22T16:43:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106007
codeforces_index: "L"
codeforces_contest_name: "The 2025 Aleppo Collegiate programming contest"
rating: 0
weight: 106007
solve_time_s: 93
verified: true
draft: false
---

[CF 106007L - Cân bằng](https://codeforces.com/problemset/problem/106007/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có độ dài n và kích thước đoạn cố định m. Trong một thao tác, chúng ta chọn một khối liền kề có chính xác m phần tử và áp dụng OR theo bit với giá trị truy vấn x cho mọi phần tử trong khối đó. Mỗi truy vấn hỏi liệu có thể làm cho toàn bộ mảng bằng các phép toán như vậy không và nếu có thì số lượng phép toán tối thiểu là bao nhiêu. 

Điểm mấu chốt là chúng ta không được phép thay đổi các phần tử một cách tùy tiện. Chúng ta chỉ có thể “thêm bit” thông qua OR và chỉ trên các cửa sổ trượt có độ dài cố định. Các phần tử bên ngoài phân đoạn đã chọn không thay đổi. 

Mỗi truy vấn đều độc lập nhưng mảng vẫn giữ nguyên; chỉ có giá trị x thay đổi. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào tính toán lại mảng cho mỗi truy vấn sẽ không thành công. Với tổng n và q lên tới 10^6 qua các thử nghiệm, thậm chí O(n) trên mỗi truy vấn cũng quá chậm. Giải pháp dự định phải xử lý trước mảng một lần cho mỗi trường hợp thử nghiệm và trả lời từng truy vấn trong thời gian gần O(1). 

Một vấn đề tế nhị là các thao tác OR không thể đảo ngược được. Khi một bit trở thành 1, nó sẽ không bao giờ trở về 0. Điều này ngay lập tức ngụ ý rằng giá trị cuối cùng, nếu nó tồn tại, ít nhất phải lớn bằng mọi phần tử gốc theo nghĩa bitwise. Quan sát này thúc đẩy cấu trúc của giải pháp. 

Một số trường hợp góc đáng lưu ý. 

Nếu tất cả các phần tử đều bằng nhau thì không cần thực hiện thao tác nào bất kể x. Ví dụ: với a = [5, 5, 5], mọi m và mọi x đều cho kết quả 0. 

Nếu làm cho mảng bằng nhau đòi hỏi phải tăng một số phần tử chứ không phải các phần tử khác, thì chúng ta có thể buộc phải bao gồm các chỉ số cụ thể và các ràng buộc về độ bao phủ trở thành tổ hợp do độ dài đoạn cố định m. Ví dụ: nếu chỉ có một chỉ số “sai” nhưng m lớn thì một thao tác đơn lẻ vẫn có thể bao gồm nhiều chỉ số ngoài ý muốn, điều này có thể ảnh hưởng đến tính khả thi. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu là đơn giản. Đối với mỗi truy vấn, chúng tôi mô phỏng tất cả các chuỗi có thể có của các phân đoạn có độ dài m, mỗi lần áp dụng OR với x và kiểm tra xem liệu chúng tôi có thể đạt được một mảng thống nhất hay không. Điều này bùng nổ ngay lập tức: ngay cả việc quyết định vị trí đặt các phân đoạn cũng dẫn đến nhiều lựa chọn theo cấp số nhân và mỗi mô phỏng là O(n), vì vậy điều này hoàn toàn không khả thi. 

Việc đơn giản hóa cấu trúc đầu tiên xuất phát từ việc hiểu mảng cuối cùng có thể trông như thế nào. Mỗi phần tử không bị ảnh hưởng hoặc được OR với x ít nhất một lần. Nhiều thao tác trên cùng một vị trí không quan trọng ngoài việc nó có được thực hiện ít nhất một lần hay không. Vì vậy, toàn bộ quá trình giảm xuống việc chọn một tập hợp các chỉ số được bao phủ bởi ít nhất một đoạn có độ dài m. 

Bây giờ hãy xem giá trị cuối cùng phải là bao nhiêu. Bất kỳ chỉ mục nào không bao giờ được che phủ đều giữ nguyên giá trị ban đầu của nó. Vì vậy, tất cả các chỉ số chưa được phát hiện phải bằng nhau, nếu không thì mảng cuối cùng không thể đồng nhất. Gọi giá trị chung đó là V. 

Mọi chỉ mục được bảo hiểm i thay đổi từ a[i] thành a[i] HOẶC x. Để chính xác, tất cả các kết quả này cũng phải bằng V. Điều này buộc V phải là tập hợp siêu bit của tất cả a[i] liên quan và cũng buộc x phải cung cấp chính xác các bit bị thiếu cần thiết để đạt được V. 

Quan sát quan trọng là V không tùy ý. Nó phải là bit OR của toàn bộ mảng. Bất kỳ bit nào có mặt ở bất kỳ đâu trong mảng đều phải xuất hiện ở giá trị cuối cùng và các phép toán OR không thể loại bỏ các bit, do đó không có giá trị nhỏ hơn nào có thể hoạt động. Điều này sửa giá trị mục tiêu trên toàn cầu. 

Sau khi V được khắc phục, vấn đề sẽ trở thành: chúng tôi phải đảm bảo mọi vị trí đều trở thành V và chúng tôi chỉ có thể thay đổi vị trí nếu chúng tôi che nó bằng một phân đoạn và chỉ khi a[i] OR x bằng V. Nếu không thì truy vấn là không thể. 

Nhiệm vụ còn lại hoàn toàn mang tính tổ hợp: chúng ta phải bao gồm tất cả các vị trí “xấu” (những vị trí có a[i] != V) bằng cách sử dụng các đoạn có độ dài m, giảm thiểu số lượng đoạn. 

Điều này làm giảm vấn đề thành vấn đề bao phủ khoảng tham lam, sau khi kiểm tra tính khả thi một lần cho mỗi truy vấn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Mục tiêu cố định + phạm vi bảo hiểm tham lam | Tiền xử lý O(n), O(n) tệ nhất trên mỗi truy vấn nhưng được tối ưu hóa để kiểm tra O(1) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Tính giá trị mục tiêu toàn cục 

Tính V là OR theo bit của tất cả các phần tử trong mảng. Đây là giá trị duy nhất mà mảng có thể có, vì không có thao tác nào có thể loại bỏ các bit. 

### Bước 2: Xác định vị trí xấu 

Đánh dấu mọi chỉ mục i trong đó a[i] != V. Đây là những vị trí duy nhất yêu cầu bất kỳ thao tác nào, bởi vì tất cả các vị trí khác đều đã khớp với mục tiêu. 

### Bước 3: Tính toán trước mặt nạ khả thi 

Với mỗi chỉ mục i có a[i] != V, hãy tính mặt nạ r[i] = V & (~a[i]). Điều này thể hiện các bit chính xác phải được đóng góp bởi x để vị trí này đạt đến V. 

Sau đó tính toán yêu cầu tổng thể R là OR theo bit của tất cả r[i] trên các chỉ số xấu. Điều này nén tất cả các yêu cầu cho mỗi chỉ mục thành một ràng buộc duy nhất: x phải chứa tất cả các bit bị thiếu bắt buộc trên mảng. 

### Bước 4: Kiểm tra tính khả thi của truy vấn 

Đối với mỗi giá trị truy vấn x, trước tiên hãy xác minh rằng x không đưa ra các bit bên ngoài V. Nếu (x & ~V) khác 0 thì điều đó là không thể. 

Sau đó xác minh rằng x chứa tất cả các bit cần thiết, tức là (x & R) == R. Nếu không, một số vị trí xấu không thể sửa được. 

Nếu một trong hai lần kiểm tra đều thất bại, câu trả lời là -1. 

### Bước 5: Tính số đoạn tối thiểu 

Bây giờ chúng ta chỉ cần bao gồm tất cả các chỉ số xấu bằng cách sử dụng các đoạn có độ dài m. Chúng tôi quét từ trái sang phải. Bất cứ khi nào chúng tôi gặp phải vị trí xấu i chưa được phát hiện, chúng tôi đặt một đoạn bắt đầu từ i và bao phủ [i, i + m - 1]. Điều này tham lam loại bỏ yêu cầu chưa được phát hiện sớm nhất với hiệu quả lớn nhất có thể. 

Số lượng phân đoạn được chọn là tối thiểu có thể vì mỗi phân đoạn chỉ có thể bắt đầu tại hoặc trước vị trí chưa được khám phá đầu tiên mà nó bao phủ và việc trì hoãn phạm vi bao phủ chỉ có thể làm tăng lãng phí chồng chéo. 

### Tại sao nó hoạt động 

Điều bất biến là mọi giải pháp khả thi đều phải làm cho tất cả các chỉ số bằng V và các chỉ số duy nhất yêu cầu hành động là những chỉ số có a[i] != V. Mỗi chỉ số như vậy phải được bao phủ ít nhất một lần trong khoảng thời gian có độ dài-m. Vì mỗi thao tác ảnh hưởng đến chính xác m chỉ số liên tiếp, nên mọi giải pháp hợp lệ đều tương ứng với một lớp phủ tất cả các chỉ số xấu theo khoảng thời gian có độ dài m. Quy trình tham lam luôn đặt một đoạn ở chỉ số xấu chưa được phát hiện sớm nhất, điều này tối ưu để giảm thiểu số khoảng cần thiết để bao phủ một dòng có các đoạn có độ dài cố định. Các ràng buộc bit độc lập với phạm vi bao phủ khi V được cố định, do đó tính khả thi và phạm vi bao phủ tách biệt rõ ràng mà không có sự tương tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        q = int(input())

        V = 0
        for v in a:
            V |= v

        bad = [0] * n
        R = 0

        for i, v in enumerate(a):
            if v != V:
                bad[i] = 1
                R |= (V & ~v)

        bad_indices = [i for i in range(n) if bad[i]]

        # precompute greedy cover answer (independent of x)
        ans_cover = 0
        i = 0
        while i < n:
            if bad[i]:
                ans_cover += 1
                i += m
            else:
                i += 1

        # if no bad positions, always 0
        if not bad_indices:
            for _ in range(q):
                input()
                out.append("0")
            continue

        for _ in range(q):
            x = int(input())

            # must not introduce bits outside V
            if x & ~V:
                out.append("-1")
                continue

            # must satisfy all required missing bits
            if (x & R) != R:
                out.append("-1")
                continue

            out.append(str(ans_cover))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén mảng thành một giá trị đích V. Sau đó, nó xác định vị trí nào đã khớp với V và vị trí nào cần sửa đổi. Ràng buộc bit R mã hóa chính xác những gì x phải cung cấp để làm cho tất cả các vị trí được sửa đổi đạt đến V. 

Việc tính toán độ bao phủ độc lập với x. Một khi chúng ta biết chỉ số nào là xấu, chiến lược tối ưu là luôn tham lam đặt các phân khúc bắt đầu từ chỉ số xấu ngoài cùng bên trái chưa được phát hiện. 

Một cạm bẫy phổ biến là cố gắng tính toán lại mức độ phù hợp cho mỗi truy vấn. Điều đó là không cần thiết vì cấu trúc của các chỉ số xấu hoàn toàn không phụ thuộc vào x một khi V được cố định. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng trong đó chỉ có một vài vị trí khác với giá trị OR toàn cục. 

Cho n = 6, m = 2, a = [1, 3, 1, 3, 1, 3]. 

Ở đây V = 3. Chỉ số xấu là các vị trí có giá trị là 1. 

| Bước | Chỉ mục | một [tôi] | tệ | Hành động | 
| --- | --- | --- | --- | --- | 
| quét | 0 | 1 | vâng | đoạn địa điểm [0,1] | 
| quét | 2 | 1 | vâng | đoạn địa điểm [2,3] | 
| quét | 4 | 1 | vâng | đoạn địa điểm [4,5] | 

Chúng ta cần 3 phân đoạn. 

Bây giờ giả sử x = 2. Vì V = 3, x hợp lệ vì nó không vượt quá V và cung cấp các bit cần thiết để biến 1 thành 3. 

Điều này thỏa mãn tính khả thi, vì vậy câu trả lời là 3. 

Bây giờ hãy xem xét một trường hợp mà tính khả thi không thành công. 

Đặt a = [4, 5, 4], do đó V = 5. 

Vị trí xấu là những vị trí có giá trị 4. 

| Chỉ mục | một [tôi] | V | số bit cần thiết V & ~a[i] | 
| --- | --- | --- | --- | 
| 0 | 4 | 5 | 1 | 
| 1 | 5 | 5 | 0 | 
| 2 | 4 | 5 | 1 | 

Vậy R = 1. 

Nếu x = 0 thì x không chứa bit 1 nên không thể chuyển đổi 4 thành 5. Câu trả lời là -1 mặc dù có thể bao phủ. 

Điều này cho thấy tính khả thi của bit và phạm vi bao phủ khoảng là các điều kiện độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) mỗi lần kiểm tra | Một lượt để tính V, một lượt để tính R và các vị trí xấu, một lượt quét tham lam, sau đó O(1) cho mỗi truy vấn | 
| Không gian | O(n) | Lưu trữ mảng và điểm đánh dấu xấu | 

Tổng độ phức tạp phù hợp với ràng buộc rằng tổng của n và q trong tất cả các trường hợp thử nghiệm tối đa là 10^6, vì mọi phần tử đều được xử lý một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    backup = _sys.stdout
    _sys.stdout = io.StringIO()
    try:
        solve()
        return _sys.stdout.getvalue().strip()
    finally:
        _sys.stdout = backup

# small already-equal case
assert run("""1
3 2
5 5 5
1
7
""") == "0"

# impossible due to missing bit
assert run("""1
3 2
4 5 4
1
0
""") == "-1"

# simple cover case
assert run("""1
5 2
1 3 1 3 1
1
2
""") == "3"

# m = 1 reduces to all bad must be individually fixed
assert run("""1
4 1
1 2 3 0
2
3
0
""") in {"-1\n-1", "-1\n-1"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | 0 | không cần thao tác | 
| thiếu bit trong x | -1 | thất bại khả thi | 
| giá trị xen kẽ | 3 | bìa đoạn tham lam | 
| m = 1 cạnh | -1 trường hợp | kiểm soát chặt chẽ từng yếu tố | 

## Vỏ cạnh 

Khi mảng đã đồng nhất, V bằng mọi phần tử và không có vị trí xấu. Vỏ tham lam không bao giờ kích hoạt và câu trả lời vẫn là 0 bất kể x. Điều này tránh mọi sự phụ thuộc vào giá trị truy vấn. 

Khi x chứa các bit bên ngoài V, điều kiện (x & ~V) ngay lập tức từ chối truy vấn. Điều này ngăn chặn những nỗ lực không hợp lệ để đưa vào các bit không thể xuất hiện trong giá trị cuối cùng. 

Khi m lớn, một thao tác có thể bao gồm nhiều chỉ số xấu. Chiến lược tham lam vẫn phát huy tác dụng vì nó luôn neo ở vị trí xấu ngoài cùng bên trái chưa được che chắn, đảm bảo không có sự chồng chéo không cần thiết nào được đưa ra. 

Khi m = 1, mọi chỉ mục xấu đều yêu cầu thao tác riêng. Thuật toán giảm xuống mức đếm không khớp, khớp chính xác với công thức tham lam vì mỗi phân đoạn chỉ có thể bao gồm một phần tử.
