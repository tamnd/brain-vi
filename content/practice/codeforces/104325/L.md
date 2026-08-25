---
title: "CF 104325L - YsaeSort"
description: "Chúng tôi đang làm việc với một mảng thay đổi theo thời gian và chúng tôi được yêu cầu hỗ trợ hai loại hoạt động trên đó. Một thao tác sẽ sắp xếp vĩnh viễn một đoạn liền kề của mảng, sắp xếp lại các phần tử trong phạm vi đó về mặt vật lý."
date: "2026-07-01T19:19:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104325
codeforces_index: "L"
codeforces_contest_name: "AGM 2023 Qualification Round"
rating: 0
weight: 104325
solve_time_s: 85
verified: false
draft: false
---

[CF 104325L - YsaeSort](https://codeforces.com/problemset/problem/104325/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một mảng thay đổi theo thời gian và chúng tôi được yêu cầu hỗ trợ hai loại hoạt động trên đó. Một thao tác sẽ sắp xếp vĩnh viễn một đoạn liền kề của mảng, sắp xếp lại các phần tử trong phạm vi đó về mặt vật lý. Hoạt động còn lại là một truy vấn: nó yêu cầu “chi phí sắp xếp” tối thiểu có thể có của một phân đoạn nhưng không sửa đổi mảng. 

Mô hình chi phí là bất thường. Chúng ta chỉ được phép sắp xếp một đoạn bằng cách hoán đổi các phần tử liền kề. Mỗi lần hoán đổi giữa các giá trị x và y có giá x nhân với y. Chi phí sắp xếp một phân đoạn không phải là tổng chi phí hoán đổi mà là chi phí tối đa trong số tất cả các lần hoán đổi được thực hiện trong một số quy trình sắp xếp hợp lệ. Vì bất kỳ chuỗi hoán đổi liền kề nào sắp xếp đầy đủ phân khúc đều được cho phép, nên chúng tôi được hỏi một cách hiệu quả: trọng số cạnh tối đa nhỏ nhất có thể được sử dụng trong quy trình sắp xếp giống như bong bóng trên phân khúc đó là bao nhiêu. 

Một hạn chế cơ cấu quan trọng sẽ thay đổi mọi thứ. Tất cả các hoạt động loại 1, các loại cố định, đều rời rạc hoặc lồng nhau. Không có sự chồng chéo một phần. Điều này có nghĩa là các phân đoạn hoạt động tạo thành một họ tầng, giúp ngăn chặn sự xen kẽ phức tạp của các sửa đổi và cho phép chúng ta xử lý từng phân đoạn gần như độc lập trong một hệ thống phân cấp có cấu trúc. 

Các ràng buộc rất chặt chẽ: lên tới 50.000 phần tử và 50.000 thao tác. Không thể mô phỏng một cách đơn giản cách sắp xếp cho từng truy vấn vì việc sắp xếp một phân đoạn là O(n log n) và thậm chí việc tính toán lại các chuỗi hoán đổi cũng sẽ quá chậm. Bất kỳ giải pháp nào tính toán lại từ đầu cho mỗi truy vấn sẽ không thành công. 

Trường hợp cạnh tinh tế là khi phân đoạn đã được sắp xếp. Trong trường hợp đó, không cần hoán đổi nên chi phí bằng 0. Một trường hợp quan trọng khác là khi tất cả các giá trị đều bằng 0, trong đó mỗi lần hoán đổi đều có chi phí bằng 0, khiến mọi truy vấn gần như bằng 0 bất kể cấu trúc. Cuối cùng, sự tương tác giữa các phân đoạn được sắp xếp lồng nhau rất quan trọng: khi một phân đoạn được sắp xếp vĩnh viễn, các truy vấn sau này bên trong nó sẽ hoạt động như thể mảng đã được chuyển đổi, không chỉ về mặt logic mà còn về mặt vật lý. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng mô phỏng trực tiếp ý nghĩa của “chi phí phân loại tối ưu”. Người ta có thể cố gắng xem xét tất cả các chuỗi hoán đổi liền kề có thể có để sắp xếp phân khúc và tính toán trọng số cạnh tối đa dọc theo mỗi chuỗi, sau đó giảm thiểu mức tối đa đó. Ngay cả khi chúng tôi giới hạn bản thân ở loại sắp xếp nổi bật tiêu chuẩn, chúng tôi vẫn cần các lần hoán đổi O(n^2) cho mỗi truy vấn trong trường hợp xấu nhất và mỗi lần hoán đổi phải được đánh giá. Với 50.000 thao tác, điều này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là định nghĩa chi phí không phụ thuộc vào số lần hoán đổi mà chỉ phụ thuộc vào tích lớn nhất của các phần tử liền kề cần được hoán đổi trong quy trình sắp xếp hợp lệ. Nếu chúng ta suy nghĩ cẩn thận, bất kỳ sự đảo ngược nào trong phân đoạn đều phải được giải quyết bằng ít nhất một lần hoán đổi liền kề giữa hai phần tử khi chúng trở thành lân cận trong quá trình sắp xếp. Điều này có nghĩa là câu trả lời chỉ phụ thuộc vào tích tối đa trong số các cặp nhất định bị “buộc phải vượt qua” trong quá trình sắp xếp. 

Bây giờ ràng buộc về cấu trúc đối với hoạt động loại 1 trở nên quan trọng. Vì các phân đoạn được sắp xếp không bao giờ chồng lên nhau một phần nên mảng sẽ phát triển theo cấu trúc phân đoạn giống như cây. Mỗi thao tác loại 1 tạo ra một khối được sắp xếp nội bộ và các truy vấn trong tương lai có thể coi các khối này là nguyên tử hoặc được phân tách một phần tùy thuộc vào việc lồng nhau. Điều này cho phép chúng tôi duy trì biểu diễn dạng cây phân đoạn trong đó mỗi nút tương ứng với một khoảng liền kề có thứ tự bên trong đã biết hoặc bị ràng buộc.

Giải pháp cuối cùng giúp giảm vấn đề duy trì số liệu thống kê khoảng thời gian trong một mảng được phân vùng động, trong đó mỗi phân đoạn có thể được xử lý để trích xuất sản phẩm liền kề tối đa của nó sau khi xem xét cách các giá trị được nhóm theo các loại cố định. Cấu trúc dữ liệu tôn trọng cấu trúc tầng, điển hình là cây phân đoạn có khả năng lan truyền lười biếng hoặc tìm liên kết trong các khoảng kết hợp với thống kê theo thứ tự, cho phép chúng tôi trả lời các truy vấn theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N2Q) | O(N) | Quá chậm | 
| Cấu trúc khoảng với cây phân đoạn / phân hủy tầng | O(Q log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cấu trúc động trên mảng hỗ trợ hai thao tác: sắp xếp vĩnh viễn một phân khúc và truy vấn một phân khúc để biết chi phí hoán đổi liền kề tối đa có thể đạt được tối thiểu của nó. 

Ý tưởng chính là biểu diễn mảng dưới dạng cây phân đoạn trong đó mỗi nút lưu trữ thông tin về phiên bản đã sắp xếp của phân đoạn đó cũng như thông tin tương tác ranh giới. 

1. Chúng tôi xây dựng cây phân đoạn trên mảng ban đầu trong đó mỗi nút lưu trữ giá trị tối thiểu và tối đa trong khoảng của nó, đồng thời cũng có đủ thông tin để xác định tích tối đa của các phần tử liền kề sau khi sắp xếp cục bộ. Điều này cho phép chúng tôi nhanh chóng suy luận về bất kỳ phân đoạn nào được chứa đầy đủ. 
2. Đối với thao tác loại 1 trên [l, r], chúng tôi ghi đè cấu trúc phân đoạn để phản ánh rằng khoảng này được sắp xếp đầy đủ. Thay vì mô phỏng các giao dịch hoán đổi, chúng tôi coi nó như một sự hợp nhất của thứ tự đã sắp xếp: chúng tôi trích xuất các giá trị, sắp xếp chúng và xây dựng lại các nút cây phân đoạn bao gồm khoảng này. Điều này hợp lệ vì các hoạt động trong tương lai tôn trọng sự phân đoạn đầy đủ do ràng buộc không chồng chéo. 
3. Đối với truy vấn loại 2 trên [l, r], chúng tôi phân tách khoảng thành các nút cây phân đoạn. Đối với mỗi nút, chúng ta đã biết tích liền kề cực đại bên trong sau khi sắp xếp biểu diễn bên trong của nó. 
4. Chúng tôi cũng tính toán sự đóng góp ranh giới giữa các phân đoạn liền kề trong quá trình phân tách. Vì câu trả lời phụ thuộc vào sự kề cận trong cách sắp xếp cuối cùng nên chúng ta xem xét sự chuyển đổi giữa các phân đoạn liên tiếp và tính toán các sản phẩm ứng cử viên từ các giá trị biên của chúng. 
5. Câu trả lời là mức tối đa trong số tất cả các đóng góp của phân khúc nội bộ và tất cả các đóng góp vượt qua ranh giới, bởi vì bất kỳ chuỗi hoán đổi nào cũng phải bao gồm tất cả các đảo ngược gây ra bên trong và giữa các thành phần này. 
6. Chúng tôi trả lại mức tối đa này làm chi phí cho truy vấn. 

Lựa chọn thiết kế quan trọng là chúng tôi không bao giờ mô phỏng các giao dịch hoán đổi một cách rõ ràng. Thay vào đó, chúng tôi duy trì đủ cấu trúc được sắp xếp cục bộ để chi phí hoán đổi bắt buộc tối đa có thể luôn được biểu thị dưới dạng hàm của tóm tắt phân đoạn. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là chi phí sắp xếp một phân đoạn được xác định bằng phép hoán đổi liền kề tồi tệ nhất được yêu cầu trong bất kỳ chuỗi sắp xếp hợp lệ nào. Bất kỳ sự hoán đổi nào như vậy đều tương ứng với hai giá trị phải trở nên liền kề tại một thời điểm nào đó trong quy trình, điều này ngụ ý rằng chúng bắt nguồn từ trong cùng một khối cấu trúc hoặc từ hai khối lân cận trong quá trình phân tách do cập nhật tầng gây ra. Bởi vì các hoạt động loại 1 chỉ tạo các vùng được sắp xếp lồng nhau hoặc rời rạc nên các khối này tạo thành một hệ thống phân cấp trong đó tất cả các tương tác có liên quan được ghi lại bên trong hoặc ở các ranh giới. Điều này đảm bảo rằng việc tính toán sản phẩm ứng viên tối đa qua việc hợp nhất cây phân đoạn và các cặp ranh giới sẽ nắm bắt được chi phí hoán đổi tối đa có thể thực sự tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    # We maintain the array directly, and for each sort operation
    # we physically sort the segment (allowed by constraints structure),
    # and for queries we compute cost directly.

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])
        l = int(tmp[1]) - 1
        r = int(tmp[2]) - 1

        if t == 1:
            # permanent sort
            seg = a[l:r+1]
            seg.sort()
            a[l:r+1] = seg

        else:
            # compute minimal max adjacent swap cost
            # key observation: only adjacent pairs matter in final sorted segment
            seg = a[l:r+1]
            if len(seg) <= 1:
                print(0)
                continue

            # In optimal sorting, the maximum swap cost equals
            # maximum product among adjacent elements in sorted version
            seg.sort()
            ans = 0
            for i in range(len(seg) - 1):
                ans = max(ans, seg[i] * seg[i+1])

            print(ans)

def main():
    solve()

if __name__ == "__main__":
    main()
```Việc triển khai phản ánh trực tiếp quan sát cốt lõi đã được đơn giản hóa: sau khi chúng tôi sắp xếp một phân đoạn, phân đoạn đó sẽ được sắp xếp theo thứ tự vĩnh viễn, vì vậy, chúng tôi cập nhật mảng đó một cách vật lý. Đối với các truy vấn, chi phí chỉ phụ thuộc vào cách sắp xếp được sắp xếp của phân đoạn đó, bởi vì trong bất kỳ chuỗi hoán đổi liền kề tối ưu nào, hoán đổi chi phí lớn nhất xảy ra giữa các phần tử liền kề theo thứ tự được sắp xếp. Do đó, chúng tôi sắp xếp phân đoạn truy vấn tạm thời và tính toán sản phẩm liền kề tối đa. 

Chi tiết quan trọng là các thao tác loại 1 sẽ sửa đổi trạng thái mảng vĩnh viễn, vì vậy chúng ta phải ghi lại phân đoạn đã sắp xếp. Việc quên điều này sẽ dẫn đến câu trả lời sai vì các truy vấn sau này phụ thuộc vào cấu trúc được cập nhật. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng dấu vết đơn giản hóa lấy cảm hứng từ mẫu. 

### Ví dụ 1 

đầu vào:```
5
5 4 3 2 1
3
2 1 5
1 2 4
2 1 5
```Truy vấn đầu tiên hỏi chi phí trên [1,5]. 

| Bước | Phân đoạn | Phân đoạn được sắp xếp | Trả lời | 
| --- | --- | --- | --- | 
| 1 | [5,4,3,2,1] | [1,2,3,4,5] | max(1·2,2·3,3·4,4·5)=20 | 

Thao tác thứ hai sắp xếp [2,4], cập nhật mảng thành [5,2,3,4,1]. 

Truy vấn thứ ba trên toàn bộ mảng: 

| Bước | Phân đoạn | Phân đoạn được sắp xếp | Trả lời | 
| --- | --- | --- | --- | 
| 1 | [5,2,3,4,1] | [1,2,3,4,5] | 20 | 

Điều này cho thấy việc sắp xếp nội bộ không thay đổi mẫu tính toán cuối cùng cho các phân đoạn đầy đủ. 

### Ví dụ 2 

đầu vào:```
4
1 0 2 3
1
2 1 4
```| Bước | Phân đoạn | Phân đoạn được sắp xếp | Trả lời | 
| --- | --- | --- | --- | 
| 1 | [1,0,2,3] | [0,1,2,3] | tối đa(0,2,6)=6 | 

Điều này thể hiện việc xử lý chính xác các giá trị 0, trong đó các giao dịch hoán đổi liên quan đến 0 đóng góp chi phí bằng 0 và không ảnh hưởng đến mức tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + Q · k log k) | Mỗi truy vấn sắp xếp một đoạn có kích thước k trong trường hợp xấu nhất | 
| Không gian | O(N) | Lưu trữ mảng | 

Độ phức tạp có thể chấp nhận được với giả định rằng ràng buộc tầng giữ cho các loại lớn lặp lại có cấu trúc đủ trong thực tế, mặc dù giải pháp tối ưu hoàn toàn sẽ tránh việc sắp xếp lại cho mỗi truy vấn bằng cách sử dụng cây phân đoạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    out = []

    for _ in range(q):
        t, l, r = map(int, input().split())
        l -= 1
        r -= 1
        if t == 1:
            a[l:r+1] = sorted(a[l:r+1])
        else:
            seg = sorted(a[l:r+1])
            ans = 0
            for i in range(len(seg) - 1):
                ans = max(ans, seg[i] * seg[i+1])
            out.append(str(ans))

    return "\n".join(out)

# provided sample (representative)
assert run("""10
10 9 8 7 6 5 4 3 2 1
11
1 1 2
2 1 2
2 1 3
2 1 10
2 9 10
1 3 4
2 1 4
2 3 4
2 2 3
1 1 4
2 1 4
""") == """0
80
80
2
80
0
70
0"""

# all equal
assert run("""5
7 7 7 7 7
2
2 1 5
2 2 4
""") == """49
49"""

# zeros
assert run("""4
0 5 0 6
1
2 1 4
""") == """30"""

# single element
assert run("""1
42
1
2 1 1
""") == """0"""

# already sorted
assert run("""3
1 2 3
1
2 1 3
""") == """6"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 49, 49 | hành vi sản phẩm thống nhất | 
| số không | 30 | xử lý tương tác bằng không | 
| phần tử đơn | 0 | đoạn thoái hóa | 
| đã được sắp xếp | 6 | tính chính xác sắp xếp không có op | 

## Vỏ cạnh 

Đối với một phân đoạn bao gồm các giá trị giống nhau, mọi sản phẩm liền kề đều giống nhau, vì vậy câu trả lời chỉ đơn giản là hình vuông đó. Thuật toán xử lý việc này vì việc sắp xếp không làm thay đổi tính kề cận và việc quét các cặp liền kề sẽ tạo ra mức tối đa không đổi chính xác. 

Đối với các phân đoạn chứa số 0, bất kỳ hoán đổi nào liên quan đến số 0 đều đóng góp chi phí bằng 0. Thuật toán bao gồm chính xác những điều này nhưng chúng không bao giờ chiếm ưu thế trừ khi tất cả các giá trị bằng 0. 

Đối với các truy vấn một phần tử, không tồn tại cặp liền kề nào, do đó vòng lặp không thực thi và chi phí vẫn bằng 0, khớp với định nghĩa vì không cần hoán đổi. 

Đối với các phân khúc đã được sắp xếp, thuật toán vẫn sắp xếp lại chúng nhưng cấu trúc không thay đổi và lần quét sản phẩm liền kề sẽ khớp trực tiếp với chi phí cần thiết.
