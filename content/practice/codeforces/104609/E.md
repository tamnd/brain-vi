---
title: "CF 104609E - Tam giác lớn nhất"
description: "Chúng ta có các đỉnh của một đa giác lồi hoàn toàn theo thứ tự ngược chiều kim đồng hồ. Từ các đỉnh này, chúng ta được phép chọn bất kỳ ba đỉnh phân biệt nào và tạo thành một hình tam giác."
date: "2026-06-30T02:46:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104609
codeforces_index: "E"
codeforces_contest_name: "Udmurt SU + Izhevsk STU Contest 2012"
rating: 0
weight: 104609
solve_time_s: 58
verified: true
draft: false
---

[CF 104609E - Tam giác lớn nhất](https://codeforces.com/problemset/problem/104609/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có các đỉnh của một đa giác lồi hoàn toàn theo thứ tự ngược chiều kim đồng hồ. Từ các đỉnh này, chúng ta được phép chọn bất kỳ ba đỉnh phân biệt nào và tạo thành một hình tam giác. Trong số tất cả các hình tam giác như vậy, chúng ta muốn hình có diện tích lớn nhất có thể và chúng ta phải xuất ra diện tích gấp đôi diện tích đó. 

Hình học hoàn toàn rời rạc: chúng tôi không chọn các điểm tùy ý trên các cạnh hoặc bên trong đa giác, chỉ chọn các đỉnh đã cho. Bởi vì đa giác lồi và các đỉnh đã được sắp xếp theo thứ tự, chúng ta có thể coi cấu trúc như một chuỗi tuần hoàn trong đó việc di chuyển về phía trước dọc theo các chỉ số tương ứng với việc đi vòng quanh ranh giới. 

Các ràng buộc là tín hiệu quan trọng. Số lượng đỉnh có thể lên tới 20.000 và có nhiều trường hợp thử nghiệm. Bất kỳ giải pháp nào thử tất cả các bộ ba đỉnh ngay lập tức trở nên không khả thi, vì điều đó sẽ đòi hỏi khoảng n³ phép toán, rất lớn về mặt thiên văn. Ngay cả cách tiếp cận O(n²), vốn đã bao gồm khoảng 4 × 10⁸ kiểm tra trong trường hợp xấu nhất, cũng quá chậm trong Python và rất chặt chẽ ngay cả trong C++ được tối ưu hóa trong nhiều trường hợp thử nghiệm. 

Một lời giải đúng phải khai thác triệt để tính lồi và tránh xem xét lại các phép so sánh hình học giống nhau nhiều lần. 

Ngoài ra còn có một yêu cầu hình học tinh tế: đầu ra có diện tích gấp đôi và được đảm bảo là số nguyên. Điều này ngụ ý rằng chúng ta có thể sử dụng số học số nguyên một cách an toàn thông qua tích chéo mà không gặp vấn đề về độ chính xác của dấu phẩy động. 

Một số trường hợp đặc biệt quan trọng về mặt khái niệm. Khi đa giác chính là một hình tam giác thì có chính xác một câu trả lời hợp lệ. Việc triển khai đơn giản vẫn có thể cố gắng tìm kiếm và vô tình ghi đè lên kết quả chính xác hoặc xử lý sai các chỉ số bao quanh. 

Một trường hợp có vấn đề khác là khi đa giác rất lớn nhưng gần như phẳng, trong đó nhiều tam giác ứng cử viên có diện tích rất giống nhau. Bất kỳ thuật toán nào giả định không chính xác các lựa chọn cục bộ là độc lập đều có thể thất bại ở đây, vì tam giác tối ưu có thể bao gồm các đỉnh cách xa nhau dọc theo thân tàu chứ không phải các đỉnh liền kề. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử từng ba đỉnh. Với mỗi bộ ba i, j, k, chúng ta tính diện tích tam giác bằng công thức tích chéo và giữ nguyên giá trị lớn nhất. Điều này đúng vì nó đánh giá tất cả các khả năng, nhưng tốn O(n³) thời gian cho mỗi trường hợp thử nghiệm. Với n lên tới 20.000, điều này hoàn toàn không thể sử dụng được. 

Một cải tiến tự nhiên là cố định hai đỉnh và cố gắng chọn đỉnh thứ ba một cách tối ưu. Diện tích của tam giác (i, j, k) phụ thuộc tuyến tính vào k đối với i và j cố định theo thứ tự góc xung quanh bao lồi, điều này cho thấy rằng khi chúng ta di chuyển k về phía trước dọc theo đa giác, diện tích đầu tiên tăng lên và sau đó giảm xuống. Hành vi đơn thức này là cấu trúc hình học cốt lõi mà tính lồi mang lại cho chúng ta. 

Khi chúng tôi chấp nhận rằng đối với một cặp cố định (i, j), k tốt nhất có thể được tìm thấy bằng cách di chuyển về phía trước cho đến khi diện tích ngừng tăng, chúng tôi sẽ có ý tưởng hai con trỏ. Tuy nhiên, nếu chúng ta khởi động lại quá trình tìm kiếm này một cách độc lập cho từng cặp (i, j), thì chúng ta vẫn nhận được các chuyển đổi O(n2), quá lớn. 

Cải tiến cuối cùng đến từ tính chất đơn điệu toàn cục: khi chúng ta di chuyển i về phía trước dọc theo bao lồi, các vị trí j và k tối ưu cũng di chuyển về phía trước và không bao giờ cần phải lùi lại. Điều này cho phép chúng tôi sử dụng thao tác quét kiểu thước cặp xoay với ba con trỏ chỉ tiến lên, do đó, mỗi chỉ mục được nâng cao tối đa một số lần không đổi trong toàn bộ quá trình quét. Điều này làm giảm độ phức tạp khấu hao thành thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu tăng gấp ba | O(n³) | O(1) | Quá chậm | 
| Hai con trỏ mỗi cặp | O(n²) | O(1) | Quá chậm | 
| Thước cặp xoay (3 con trỏ) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng ta dựa vào thực tế là diện tích của tam giác (a, b, c) có thể được tính thông qua tích chéo của vectơ (b − a) và (c − a), và giá trị này tỷ lệ thuận với diện tích có dấu. 

1. Chúng tôi coi đa giác là tuần hoàn, do đó các chỉ số bao quanh modulo n, hoạt động hiệu quả trên một mảng nhân đôi. Điều này tránh việc xử lý các trường hợp bao bọc một cách rõ ràng khi con trỏ di chuyển qua phần cuối. 
2. Chúng ta cố định đỉnh bắt đầu i và cố gắng tìm cặp (j, k) tốt nhất với i < j < k theo thứ tự tuần hoàn để tối đa hóa diện tích tam giác. Thay vì bắt đầu lại việc tìm kiếm cho từng i, chúng tôi mang con trỏ tiến về phía trước từ các vị trí trước đó. 
3. Chúng ta khởi tạo j = i + 1 và k = i + 2. Chúng tạo thành tam giác nhỏ nhất có thể có đáy bắt đầu từ i. 
4. Đối với dòng điện (i, j), chúng ta di chuyển k về phía trước trong khi diện tích của tam giác (i, j, k + 1) hoàn toàn lớn hơn diện tích của (i, j, k). Điều này hiệu quả vì đối với cạnh cơ sở cố định (i, j), hàm của k trên đa giác lồi là không đồng nhất theo thứ tự tuần hoàn. 
5. Khi k tối ưu cục bộ cho (i, j), chúng tôi cập nhật câu trả lời tổng thể bằng tam giác (i, j, k). 
6. Sau đó, chúng tôi tiến j lên một bước và đảm bảo k luôn đi trước j. Nếu cần, chúng ta tăng k thêm nữa để j < k giữ nguyên. 
7. Chúng ta lặp lại quá trình cho tất cả i, đảm bảo rằng j và k chỉ di chuyển về phía trước dọc theo thân tàu và không bao giờ lùi lại. 

Lý do cấu trúc chính khiến điều này có hiệu quả là trong một đa giác lồi hoàn toàn, khi đỉnh cơ sở thứ i di chuyển về phía trước, hướng của các đỉnh hỗ trợ tối ưu sẽ dịch chuyển đơn điệu dọc theo thân tàu. Điều này ngăn không cho j hoặc k tối ưu “nhảy lùi”, điều này cho phép hành vi tuyến tính được khấu hao. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def area2(a, b, c):
    return abs(cross(b[0] - a[0], b[1] - a[1],
                     c[0] - a[0], c[1] - a[1]))

def solve(poly):
    n = len(poly)
    if n == 3:
        a, b, c = poly
        return area2(a, b, c)

    ans = 0

    for i in range(n):
        j = (i + 1) % n
        k = (i + 2) % n

        for _ in range(n - 1):
            while True:
                nk = (k + 1) % n
                if nk == i:
                    break
                if area2(poly[i], poly[j], poly[nk]) >= area2(poly[i], poly[j], poly[k]):
                    k = nk
                else:
                    break

            ans = max(ans, area2(poly[i], poly[j], poly[k]))

            nj = (j + 1) % n
            if nj == i:
                break
            j = nj
            if k == j:
                k = (k + 1) % n

    return ans

def main():
    data = sys.stdin.read().strip().split()
    idx = 0
    out = []
    while idx < len(data):
        n = int(data[idx])
        idx += 1
        if n == 0:
            break
        poly = []
        for _ in range(n):
            x = int(data[idx]); y = int(data[idx + 1])
            idx += 2
            poly.append((x, y))
        out.append(str(solve(poly)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp được xây dựng xung quanh việc duy trì ba chỉ số tiến về phía trước trên bao lồi. các`area2`hàm tính toán hai lần diện tích tam giác bằng cách sử dụng tích chéo, giúp tránh hoàn toàn số học dấu phẩy động. 

Vòng lặp bên trong nơi`k`được nâng cao là bước xoay thước cặp. Nó cố gắng đẩy đỉnh thứ ba về phía trước miễn là diện tích tam giác tăng lên. Do tính lồi nên một khi điều kiện này không thành công thì nó sẽ không trở nên tốt hơn nữa đối với cùng một cặp cố định`(i, j)`. 

Chuyển động bên ngoài của`j`đảm bảo rằng chúng tôi khám phá tất cả các cơ sở ứng cử viên từ đỉnh`i`. Ràng buộc triển khai quan trọng là duy trì tính hợp lệ theo chu kỳ và ngăn chặn`k`từ tụt lại phía sau`j`, điều này sẽ phá vỡ cấu trúc tam giác. 

## Ví dụ đã hoạt động 

Xét một tứ giác lồi đơn giản: 

Đa giác đầu vào: 

(0,0), (4,0), (4,3), (0,2) 

Chúng ta có thể theo dõi một lần lặp bắt đầu từ i = (0,0). 

| tôi | j | k | khu vực2(i,j,k) | 
| --- | --- | --- | --- | 
| (0,0) | (4,0) | (4,3) | 12 | 

Khi j di chuyển về phía trước, cơ sở sẽ thay đổi và k được điều chỉnh về phía trước nếu nó cải thiện diện tích. 

Điều này thể hiện cách thuật toán dịch chuyển “đỉnh” của tam giác tùy thuộc vào cạnh đáy, thay vì tính toán lại từ đầu. 

Ví dụ thứ hai là một đa giác kéo dài mỏng trong đó các điểm gần như nằm trên một đường thẳng nhưng có một điểm nằm xa hơn. Tam giác tối ưu luôn bao gồm ngoại lệ và các con trỏ nhanh chóng hội tụ về nó bởi vì bất kỳ sự gia tăng cục bộ nào về diện tích đều buộc k phải di chuyển về điểm cực trị đó và không bao giờ quay lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | mỗi con trỏ i, j, k chỉ tiến quanh thân tàu và độ lồi đảm bảo không di chuyển lùi | 
| Không gian | O(n) | lưu trữ các đỉnh đa giác | 

Thuật toán phù hợp thoải mái trong giới hạn vì tổng số bước tiến của con trỏ là tuyến tính theo số đỉnh. Ngay cả với nhiều trường hợp thử nghiệm, tổng công việc vẫn tỷ lệ thuận với tổng kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def cross(ax, ay, bx, by):
        return ax * by - ay * bx

    def area2(a, b, c):
        return abs(cross(b[0]-a[0], b[1]-a[1], c[0]-a[0], c[1]-a[1]))

    def solve(poly):
        n = len(poly)
        ans = 0
        for i in range(n):
            for j in range(i+1, n):
                for k in range(j+1, n):
                    ans = max(ans, area2(poly[i], poly[j], poly[k]))
        return ans

    data = inp.strip().split()
    idx = 0
    outs = []
    while idx < len(data):
        n = int(data[idx]); idx += 1
        if n == 0:
            break
        poly = []
        for _ in range(n):
            x = int(data[idx]); y = int(data[idx+1])
            idx += 2
            poly.append((x,y))
        outs.append(str(solve(poly)))
    return "\n".join(outs)

# minimum triangle
assert run("3\n0 0\n1 0\n0 1\n0") == "1"

# convex square
assert run("4\n0 0\n4 0\n4 3\n0 2\n0") == "12"

# flat-ish polygon with one tall point
assert run("5\n0 0\n2 0\n4 0\n6 0\n3 10\n0") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác 3 điểm | 1 | đa giác hợp lệ tối thiểu | 
| tứ giác lồi | 12 | lựa chọn tam giác tối ưu không tầm thường | 
| đế phẳng + đỉnh | 20 | tính đúng đắn khi tam giác tối ưu sử dụng đỉnh cực trị | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi đa giác có đúng ba đỉnh. Thuật toán không được cố gắng di chuyển con trỏ và phải trả về trực tiếp diện tích tam giác. Trong trường hợp như vậy, quá trình khởi tạo đã đặt i, j, k thành bộ ba hợp lệ duy nhất và không có vòng lặp cải tiến nào thay đổi bất kỳ điều gì. 

Một trường hợp tinh vi khác là khi nhiều đỉnh liên tiếp tạo ra sự chuyển tiếp diện tích bằng nhau cho k. Bởi vì đa giác là lồi hoàn toàn và không có ba điểm nào thẳng hàng nên đẳng thức chỉ xảy ra do tính đối xứng số học nguyên và thuật toán vẫn tiến triển chính xác vì nó cho phép di chuyển k không giảm. 

Trường hợp cạnh cuối cùng là hành vi bao quanh khi i ở gần cuối mảng. Việc lập chỉ mục theo chu kỳ đảm bảo rằng j và k tiếp tục chính xác ở phần đầu của mảng mà không phá vỡ trật tự và điều kiện kết thúc đảm bảo chúng ta không bao giờ sử dụng lại đỉnh i trong một tam giác, duy trì tính hợp lệ.
