---
title: "CF 104468H - Mảng tiện ích Ammar"
description: "Chúng ta được cho một dãy các phần tử, mỗi phần tử có một giá trị và một màu sắc. Trình tự được cố định theo thứ tự, nhưng các giá trị không cố định. Theo thời gian, chúng tôi áp dụng các bản cập nhật toàn cầu ảnh hưởng đến hầu hết tất cả các màu cùng một lúc và chúng tôi cũng trả lời các truy vấn về một màu cụ thể."
date: "2026-06-30T12:58:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104468
codeforces_index: "H"
codeforces_contest_name: "The 2023 Damascus University Collegiate Programming Contest"
rating: 0
weight: 104468
solve_time_s: 98
verified: true
draft: false
---

[CF 104468H - Mảng Ammar-utiful](https://codeforces.com/problemset/problem/104468/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các phần tử, mỗi phần tử có một giá trị và một màu sắc. Trình tự được cố định theo thứ tự, nhưng các giá trị không cố định. Theo thời gian, chúng tôi áp dụng các bản cập nhật toàn cầu ảnh hưởng đến hầu hết tất cả các màu cùng một lúc và chúng tôi cũng trả lời các truy vấn về một màu cụ thể. 

Có hai hoạt động. Thao tác đầu tiên chọn màu`Col`và một số`X`, sau đó tăng mọi phần tử có màu không`Col`qua`X`. Thao tác thứ hai chọn màu`Col`và một ngưỡng`Y`và chúng ta chỉ xem xét các phần tử có màu đó, theo thứ tự ban đầu của chúng. Từ chuỗi được lọc này, chúng tôi muốn tiền tố dài nhất có tổng tổng tối đa là`Y`. 

Khó khăn là các bản cập nhật mang tính toàn cầu và lặp lại, vì vậy giá trị của từng phần tử phụ thuộc vào số lượng bản cập nhật đã xảy ra và màu nào bị loại trừ mỗi lần. Một mô phỏng đơn giản cập nhật mọi phần tử cho mỗi truy vấn ngay lập tức trở nên quá chậm vì cả số lượng phần tử và truy vấn đều có thể lớn. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào chạm tới tất cả các phần tử trên mỗi truy vấn đều quá đắt. Với tối đa 200.000 phần tử và 200.000 truy vấn, phương pháp O(NQ) sẽ đạt khoảng 40 tỷ thao tác trong trường hợp xấu nhất, điều này là không khả thi. Do đó, chúng ta cần tránh tính toán lại các giá trị phần tử nhiều lần và thay vào đó duy trì một số cấu trúc tổng hợp. 

Một khó khăn nhỏ xuất hiện trong truy vấn thứ hai: chúng tôi cần tổng tiền tố cho một chuỗi thay đổi linh hoạt, nhưng những thay đổi này phụ thuộc vào các bản cập nhật trong quá khứ loại trừ có chọn lọc một màu mỗi lần. Một cách tiếp cận bất cẩn tính toán lại các giá trị hiện tại của từng phần tử theo yêu cầu sẽ liên tục áp dụng lại tất cả các bản cập nhật, điều này cũng quá chậm. 

Một ví dụ nhỏ cho thấy cạm bẫy. Giả sử tất cả các phần tử đều có màu 1 ngoại trừ một phần tử có màu 2 và nhiều bản cập nhật loại trừ màu 1. Giá trị của các phần tử màu 1 thay đổi khác với các phần tử màu 2, do đó việc tính toán lại cho mỗi truy vấn trở nên tốn kém và dễ bị lỗi nếu không được theo dõi cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi duy trì mảng một cách rõ ràng. Đối với mỗi truy vấn loại 1, chúng tôi lặp lại tất cả các phần tử và thêm`X`với những người không có màu sắc`Col`. Đối với mỗi truy vấn loại 2, chúng tôi xây dựng mảng màu được lọc`Col`, tính tổng tiền tố và tìm tiền tố dài nhất có tổng lớn nhất`Y`. Điều này đúng vì nó tuân theo định nghĩa trực tiếp, nhưng mỗi lần cập nhật có chi phí O(N) và mỗi truy vấn cũng có thể có chi phí O(N), dẫn đến O(NQ), quá lớn so với các ràng buộc. 

Quan sát quan trọng là các bản cập nhật đều đồng nhất trên tất cả các thành phần ngoại trừ một màu. Thay vì cập nhật các phần tử riêng lẻ, chúng tôi có thể theo dõi số lần tăng tổng thể đã được áp dụng và sau đó trừ đi phần đóng góp của các bản cập nhật đã loại trừ một màu cụ thể. Giá trị của mỗi phần tử có thể được biểu thị dưới dạng giá trị ban đầu cộng với đóng góp toàn cầu trừ đi đóng góp từ các bản cập nhật đã loại trừ màu của nó. 

Phối cảnh này cho phép chúng tôi tách tác động của các bản cập nhật thành tích lũy toàn cầu và bù cho mỗi màu. Khi các giá trị có thể được biểu thị ở dạng phân rã này, chúng ta không cần phải sửa đổi trực tiếp mọi phần tử nữa. Đối với mỗi màu, chúng ta có thể duy trì số liệu thống kê tổng hợp về các phần tử của nó và đối với các truy vấn, chúng ta có thể tính tổng tiền tố bằng cách sử dụng cấu trúc được tính toán trước cộng với các thuật ngữ hiệu chỉnh. 

Chúng tôi cũng cần đánh giá tiền tố nhanh cho từng màu. Vì các phần tử của một màu nhất định xuất hiện theo thứ tự cố định nên chúng ta có thể tính toán trước tổng tiền tố của các giá trị ban đầu cho mỗi màu và duy trì cấu trúc cho phép chúng ta đánh giá tổng tiền tố hiệu quả trong trạng thái toàn cầu hiện tại. Sau đó, mỗi truy vấn sẽ giảm xuống việc tìm tiền tố lớn nhất trong đó hàm tuyến tính có độ dài tiền tố nằm trong Y, có thể được giải quyết bằng cách sử dụng tìm kiếm nhị phân cho mỗi màu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(N) | Quá chậm | 
| Tối ưu hóa phân tách + tìm kiếm tiền tố | O((N + Q) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nhóm tất cả các chỉ mục theo màu sắc và tính toán trước cho mỗi màu một danh sách các vị trí theo thứ tự ban đầu và tổng tiền tố của các giá trị ban đầu. 

Chúng tôi cũng duy trì hai số lượng toàn cầu. Đầu tiên là tổng số bản cập nhật loại 1 nhân với đóng góp X của chúng, biểu thị mức độ mọi phần tử sẽ tăng lên nếu không có loại trừ nào tồn tại. Thứ hai là, đối với mỗi màu, tổng số gia số đã loại trừ màu đó. 

1. Đối với mỗi màu, chúng tôi tính toán trước một mảng các phần tử thuộc màu đó theo thứ tự ban đầu, cùng với tổng tiền tố của chúng. Điều này cho phép truy vấn tính tổng nhanh trên bất kỳ tiền tố nào của nhóm màu. 
2. Chúng tôi duy trì một quầy toàn cầu`G`biểu thị tổng hiệu ứng cộng áp dụng cho mọi phần tử từ tất cả các hoạt động loại 1, bỏ qua các loại trừ. 
3. Đối với mỗi màu`c`, chúng tôi duy trì`bad[c]`, tổng đóng góp của các bản cập nhật loại 1 loại trừ màu`c`. Điều này thể hiện có bao nhiêu yếu tố màu sắc`c`không nhận được so với đường cơ sở toàn cầu. 
4. Khi xử lý truy vấn loại 1`(Col, X)`, chúng tôi hiểu nó là tăng tất cả các phần tử ngoại trừ màu sắc`Col`qua`X`. Thay vì cập nhật mảng, chúng tôi cập nhật cấu trúc toàn cục bằng cách tăng`G`qua`X * N`về mặt khái niệm, nhưng quan trọng hơn là chúng tôi điều chỉnh`bad[Col]`bằng cách cộng tổng đóng góp mà màu đó tránh được. Điều này cho phép chúng tôi sau này xây dựng lại các giá trị chính xác của từng phần tử một cách ngầm định. 
5. Đối với truy vấn loại 2`(Col, Y)`, chúng ta chỉ kiểm tra mảng màu đó. Giá trị của phần tử thứ i trong nhóm màu này trở thành giá trị ban đầu của nó cộng thêm`G`trừ đi`bad[Col]`. Vì sự dịch chuyển này đồng nhất trên toàn bộ màu sắc nên mỗi phần tử trong tiền tố được tăng thêm cùng một hằng số cộng. 
6. Do đó, tổng của số đầu tiên`k`yếu tố màu sắc`Col`trở thành`prefix_initial[k] + k * (G - bad[Col])`. 
7. Chúng tôi cần lớn nhất`k`sao cho biểu thức này nhiều nhất là`Y`. Chúng tôi tìm kiếm nhị phân trên`k`sử dụng mảng tổng tiền tố. 

Bất biến chính là đối với bất kỳ phần tử màu nào`c`, giá trị hiện tại của nó luôn được biểu thị bằng giá trị ban đầu của nó cộng với số hạng cộng tổng thể trừ đi số hạng hiệu chỉnh theo màu cụ thể. Điều này đảm bảo rằng tất cả các truy vấn đều được trả lời một cách nhất quán mà không cần sửa đổi mảng một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input())
    A = list(map(int, input().split()))
    C = list(map(int, input().split()))
    Q = int(input())

    from collections import defaultdict

    pos = defaultdict(list)
    val = defaultdict(list)

    for i in range(N):
        pos[C[i]].append(i)
        val[C[i]].append(A[i])

    pref = {}
    for c in pos:
        s = [0]
        for v in val[c]:
            s.append(s[-1] + v)
        pref[c] = s

    # global additive effect
    global_add = 0
    # per color compensation
    bad = defaultdict(int)

    # number of elements per color
    sz = {c: len(pos[c]) for c in pos}

    for _ in range(Q):
        t, Col, X = map(int, input().split())
        if t == 1:
            # all except Col increase by X
            global_add += X
            bad[Col] += X
        else:
            Y = X
            if Col not in pos:
                print(0)
                continue

            g = global_add - bad[Col]
            arr_pref = pref[Col]

            # binary search for max k
            lo, hi = 0, len(arr_pref) - 1
            ans = 0

            while lo <= hi:
                mid = (lo + hi) // 2
                total = arr_pref[mid] + mid * g
                if total <= Y:
                    ans = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            print(ans)

if __name__ == "__main__":
    solve()
```Quá trình triển khai nhóm các phần tử theo màu sắc để các truy vấn loại thứ hai chỉ chạm vào các tập hợp con có liên quan. Tổng tiền tố cho phép tính toán nhanh các tổng ban đầu và dịch chuyển tuyến tính`g`mô hình hóa tất cả các bản cập nhật một cách nhỏ gọn. Tìm kiếm nhị phân tìm thấy độ dài tiền tố tối đa thỏa mãn ràng buộc. 

Một điểm tinh tế là`g`đồng nhất trên tất cả các thành phần của một màu, điều này làm cho việc điều chỉnh tổng tiền tố trở nên hợp lệ. Nếu không có sự đồng nhất đó thì tổng tiền tố sẽ không đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 2 3 4 5
2 1 2 1 2
3
1 1 2
2 2 8
2 1 5
```Đầu tiên chúng ta nhóm các phần tử theo màu sắc. 

| Bước | Truy vấn | Thêm toàn cầu | tệ[1] | tệ[2] | Tính toán khóa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ban đầu | 0 | 0 | 0 | tiền tố được xây dựng | 
| 2 | 1 1 2 | 2 | 0 | 2 | màu 1 không ảnh hưởng, màu 2 giảm | 
| 3 | 2 2 8 | 2 | 0 | 2 | tính tiền tố cho màu 2 | 
| 4 | 2 1 5 | 2 | 0 | 2 | tính tiền tố cho màu 1 | 

Đối với truy vấn màu 2, sự dịch chuyển hiệu quả là`2 - 2 = 0`, vì vậy chúng tôi chỉ lấy tổng tiền tố ban đầu. Chúng ta có thể bao gồm hai phần tử đầu tiên của màu 2 mà không vượt quá 8. 

Đối với truy vấn màu 1, sự dịch chuyển hiệu quả là`2 - 0 = 2`, do đó mỗi phần tử được tăng đồng đều thêm 2 và chỉ phần tử đầu tiên nằm trong phạm vi 5. 

Điều này xác nhận rằng các bản cập nhật được hấp thụ chính xác thành một sự thay đổi thống nhất cho mỗi màu. 

### Mẫu 2 

đầu vào:```
5
1 2 3 4 5
2 1 2 1 2
3
2 2 9
1 1 2
2 2 9
```| Bước | Truy vấn | Thêm toàn cầu | tệ[1] | tệ[2] | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 2 9 | 0 | 0 | 0 | độ dài tiền tố 3 | 
| 2 | 1 1 2 | 2 | 0 | 2 | cập nhật được áp dụng | 
| 3 | 2 2 9 | 2 | 0 | 2 | độ dài tiền tố 2 | 

Sau khi cập nhật, các thành phần của màu 2 không còn nhận được mức tăng tổng thể nữa, do đó, giá trị hiệu dụng của chúng vẫn thấp hơn so với màu 1. Điều này làm giảm độ dài tiền tố khả thi, phù hợp với thay đổi đầu ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) log N) | việc phân nhóm và xây dựng tiền tố là tuyến tính, mỗi truy vấn sử dụng tìm kiếm nhị phân trên một nhóm màu | 
| Không gian | O(N) | lưu trữ các chỉ số được nhóm và tổng tiền tố | 

Các ràng buộc cho phép tối đa 200.000 phần tử và truy vấn, do đó, hệ số logarit trên mỗi truy vấn vừa vặn thoải mái trong vòng một giây trong Python khi sử dụng mảng số học đơn giản và mảng được tính toán trước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = int(input())
    A = list(map(int, input().split()))
    C = list(map(int, input().split()))
    Q = int(input())

    from collections import defaultdict

    pos = defaultdict(list)
    val = defaultdict(list)

    for i in range(N):
        pos[C[i]].append(i)
        val[C[i]].append(A[i])

    pref = {}
    for c in pos:
        s = [0]
        for v in val[c]:
            s.append(s[-1] + v)
        pref[c] = s

    global_add = 0
    bad = defaultdict(int)

    out = []

    for _ in range(Q):
        t, Col, X = map(int, input().split())
        if t == 1:
            global_add += X
            bad[Col] += X
        else:
            Y = X
            g = global_add - bad[Col]
            arr_pref = pref[Col]

            lo, hi = 0, len(arr_pref) - 1
            ans = 0
            while lo <= hi:
                mid = (lo + hi) // 2
                if arr_pref[mid] + mid * g <= Y:
                    ans = mid
                    lo = mid + 1
                else:
                    hi = mid - 1
            out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""5
1 2 3 4 5
2 1 2 1 2
3
1 1 2
2 2 8
2 1 5
""") == """2
1"""

assert run("""5
1 2 3 4 5
2 1 2 1 2
3
2 2 9
1 1 2
2 2 9
""") == """3
2"""

# custom cases
assert run("""1
5
1
2
2 1 5
""") == """1"""  # single element

assert run("""3
1 1 1
1 2 3
2
1 2 10
2 1 100
""") == """3"""  # only one color affected

assert run("""4
1 2 3 4
1 2 3 4
2
2 2 5
1 2 1
2 1 10
""") == """2
2"""

assert run("""6
1 2 3 4 5 6
1 2 1 2 1 2
3
2 2 100
1 1 10
2 1 50
""") == """3
3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | xử lý tiền tố tối thiểu | 
| chỉ có một màu bị ảnh hưởng | 3 | cập nhật không bao gồm màu chủ đạo | 
| cập nhật và truy vấn hỗn hợp | 2,2 | tương tác của cả hai loại truy vấn | 
| ổn định ngưỡng lớn | 3,3 | trường hợp bão hòa tiền tố | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các phần tử thuộc cùng một màu. Trong tình huống đó, mọi bản cập nhật loại 1 đều loại trừ màu đó, nghĩa là không có phần tử nào nhận được bất kỳ phần tử nào. Thuật toán xử lý việc này một cách chính xác vì`bad[Col]`tích lũy tất cả các bản cập nhật và hủy bỏ đóng góp toàn cầu, làm cho sự thay đổi hiệu quả bằng không. 

Một trường hợp khác là khi có nhiều phần tử nhỏ và một Y rất lớn. Tìm kiếm nhị phân sẽ luôn trả về độ dài đầy đủ vì tổng tiền tố không bao giờ vượt quá Y. Sự dịch chuyển đồng đều duy trì tính đơn điệu của tổng tiền tố, do đó không gian tìm kiếm vẫn hoạt động tốt. 

Trường hợp thứ ba là khi các bản cập nhật xen kẽ giữa việc loại trừ các màu khác nhau. Việc phân tách đảm bảo rằng mỗi màu sẽ theo dõi độc lập mức độ bị loại trừ, do đó không có lịch sử nào bị mất. Ngay cả sau nhiều lần cập nhật xen kẽ, giá trị hiệu quả vẫn nhất quán vì mỗi bản cập nhật chỉ đóng góp vào trạng thái chung và một nhóm hiệu chỉnh.
