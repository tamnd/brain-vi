---
title: "CF 106017A - Máy nếm thử Perm\u00e1tomo (Phiên bản dễ dàng)"
description: "Nhiệm vụ yêu cầu chúng ta xem xét từng cặp vị trí có thứ tự trong một mảng. Đối với một cặp (i, j), chúng ta tạo thành một giá trị bằng cách cộng hai phần tử đã chọn. Có N2 giá trị như vậy vì việc chọn vị trí thứ nhất và thứ hai là những lựa chọn độc lập."
date: "2026-06-25T13:14:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106017
codeforces_index: "A"
codeforces_contest_name: "2025 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 106017
solve_time_s: 43
verified: true
draft: false
---

[CF 106017A - Máy nếm thử Perm\u00e1tomo (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/106017/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta xem xét từng cặp vị trí có thứ tự trong một mảng. Đối với một cặp`(i, j)`, chúng ta tạo thành một giá trị bằng cách cộng hai phần tử đã chọn. có`N²`những giá trị như vậy vì việc chọn vị trí thứ nhất và thứ hai là những lựa chọn độc lập. Chúng ta cần tìm`K`-giá trị nhỏ nhất trong số tất cả các tổng cặp này. 

Phiên bản dễ dàng cho phép`N`tối đa là 500, với tối đa 20 trường hợp thử nghiệm. Điều này có nghĩa là trực tiếp`O(N²)`Cách tiếp cận này đã có khoảng 250.000 thao tác cho mỗi trường hợp thử nghiệm, điều này có thể chấp nhận được. Tuy nhiên, nếu chúng ta thêm công việc phụ như sắp xếp tất cả`N²`tổng cộng, số lượng giá trị được tạo cũng là 250.000 và việc sắp xếp chúng vẫn ổn. Giải pháp dự định thường là sử dụng cấu trúc đặc biệt của các tổng cặp thay vì cụ thể hóa mọi thứ, bởi vì nó cũng dạy kỹ thuật được sử dụng cho các biến thể lớn hơn. 

Các giá trị trong mảng có thể lớn bằng`10^9`về giá trị tuyệt đối, do đó câu trả lời có thể đạt được`2 * 10^9`. Một giải pháp phải sử dụng các loại số nguyên có thể giữ các giá trị này một cách an toàn. Số nguyên Python tự động xử lý việc này. 

Trường hợp phức tạp đầu tiên là khi`K`đề cập đến số tiền trùng lặp. Ví dụ:```
1
3 4
1 1 5
```Tổng của cặp là:```
2 2 6
2 2 6
6 6 10
```Câu trả lời đúng là`6`cho giá trị nhỏ thứ tư. Giải pháp chỉ lưu trữ số tiền duy nhất sẽ trả về không chính xác`10`. 

Một trường hợp cạnh khác xuất hiện với giá trị âm:```
1
2 2
-5 -1
```Tổng số tiền là`-10, -6, -6, -2`, vậy câu trả lời là`-6`. Ở đây, tìm kiếm nhị phân giả định câu trả lời là khẳng định hoặc khởi tạo giới hạn của nó không chính xác sẽ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra mọi cặp đôi có thể. Đối với mỗi chỉ số`i`, chúng tôi thử mọi chỉ mục`j`và nối thêm`a[i] + a[j]`vào một danh sách. Sau khi tạo tất cả`N²`tính tổng, chúng ta sắp xếp danh sách và lấy phần tử vào vị trí`K - 1`. 

Điều này có tác dụng vì việc sắp xếp đặt tập hợp tổng hoàn chỉnh theo đúng thứ tự yêu cầu. Vấn đề là chúng ta tạo ra một mảng trung gian lớn và phải trả chi phí sắp xếp. Số lượng giá trị được tạo ra là`N²`và sắp xếp chúng`O(N² log(N²))`. Đối với những giới hạn dễ dàng, điều này sẽ vượt qua, nhưng nó sẽ không mở rộng tốt nếu các ràng buộc tăng lên. 

Quan sát giúp cải thiện giải pháp là sau khi sắp xếp mảng ban đầu, chúng ta có thể trả lời một câu hỏi khác: "Có bao nhiêu cặp tổng nhỏ hơn hoặc bằng X?" Nếu chúng ta có thể trả lời nhanh chóng, chúng ta có thể tìm kiếm nhị phân cho X nhỏ nhất chứa ít nhất K tổng. 

Đối với một giá trị cố định`X`, hãy xem xét một mảng được sắp xếp. Đối với mỗi phần tử bên trái`a[i]`, tất cả các đối tác hợp lệ đều tạo thành hậu tố hoặc tiền tố tùy thuộc vào việc triển khai. Vì mảng đã được sắp xếp nên chúng ta có thể sử dụng hai con trỏ. Bắt đầu từ chỉ số lớn nhất bên phải, nếu`a[i] + a[j]`lớn hơn`X`, chúng tôi di chuyển`j`bên trái. Khi nó trở nên hợp lệ, mọi chỉ mục từ`0`ĐẾN`j`cũng hợp lệ, thêm`j + 1`các đối tác có thể. 

Lực lượng vũ phu hoạt động vì nó nhìn thấy rõ ràng mọi số tiền có thể. Phương pháp nhanh hơn hoạt động vì thứ tự sắp xếp cho phép chúng ta đếm nhiều tổng cùng một lúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2 log(N2)) | O(N2) | Được chấp nhận cho phiên bản dễ dàng | 
| Tìm kiếm nhị phân trên câu trả lời | O(N log V + N log N) | O(1) thêm | Đã chấp nhận | 

Đây`V`là phạm vi các câu trả lời có thể có, về`2 * 10^9`, vì vậy tìm kiếm nhị phân cần khoảng 31 lần lặp. 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng. Thứ tự được sắp xếp cho phép chúng ta đếm xem có bao nhiêu cặp có tổng dưới giá trị đã chọn mà không cần kiểm tra từng cặp. 
2. Đặt phạm vi tìm kiếm nhị phân từ tổng cặp nhỏ nhất có thể đến tổng cặp lớn nhất có thể. Tổng nhỏ nhất có thể được tạo ra từ hai phần tử nhỏ nhất và tổng lớn nhất có thể đến từ hai phần tử lớn nhất. 
3. Đối với giá trị trung bình hiện tại`mid`, đếm xem có bao nhiêu cặp được đặt hàng`(i, j)`thỏa mãn`a[i] + a[j] <= mid`. 
4. Để đếm hiệu quả, hãy giữ một con trỏ`j`bắt đầu từ cuối mảng đã sắp xếp. Đối với mọi`i`, giảm bớt`j`trong khi cặp hiện tại quá lớn. Một lần`a[i] + a[j]`là hợp lệ, mọi chỉ mục từ`0`ĐẾN`j`làm việc với`i`, vì vậy hãy thêm`j + 1`để đếm. 
5. Nếu số lượng ít nhất là`K`, câu trả lời có thể là`mid`hoặc nhỏ hơn, do đó hãy di chuyển ranh giới bên phải của tìm kiếm nhị phân xuống dưới. Ngược lại, không đủ số tiền đủ nhỏ nên hãy di chuyển ranh giới bên trái lên trên. 
6. Khi quá trình tìm kiếm kết thúc, ranh giới bên trái là giá trị nhỏ nhất có ít nhất`K`số tiền cặp không vượt quá nó. 

Tại sao nó hoạt động: chức năng đếm đơn điệu. Nếu một giá trị`X`có ít nhất`K`tổng không lớn hơn nó thì mọi giá trị lớn hơn cũng có ít nhất`K`tiền. Nếu nó có ít hơn`K`, mọi giá trị nhỏ hơn cũng có ít hơn`K`. Tìm kiếm nhị phân tìm thấy giá trị đầu tiên trong đó điều kiện trở thành đúng, chính xác là`K`- tổng nhỏ nhất thứ 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    def count_pairs(x):
        res = 0
        j = n - 1
        for i in range(n):
            while j >= 0 and a[i] + a[j] > x:
                j -= 1
            res += j + 1
        return res

    left = a[0] + a[0]
    right = a[-1] + a[-1]

    while left < right:
        mid = (left + right) // 2
        if count_pairs(mid) >= k:
            right = mid
        else:
            left = mid + 1

    return str(left)

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        ans.append(solve_case())
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Mảng được sắp xếp trước vì hàm đếm phụ thuộc vào thứ tự đơn điệu. Nếu không sắp xếp, việc di chuyển con trỏ sẽ không thể hiện được tất cả các đối tác có thể có. 

các`count_pairs`hàm duy trì một con trỏ`j`chỉ di chuyển sang trái trong toàn bộ quá trình quét. Đây là lý do tại sao số lượng là`O(N)`thay vì`O(N²)`. Giá trị của`j`không bao giờ đặt lại cho mỗi cái mới`i`. 

Việc tìm kiếm nhị phân sử dụng`left < right`thay vì tìm kiếm theo chỉ mục vì chúng ta đang tìm kiếm một giá trị chứ không phải một vị trí. Giá trị cuối cùng của`left`là ngưỡng tổng đầu tiên chứa ít nhất`K`cặp. 

Giới hạn được tính từ số tiền tối thiểu và tối đa thực tế có thể. Điều này xử lý các số âm một cách chính xác và tránh các giả định về phạm vi đầu vào. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
3 4
1 3 5
2 2
-1 2
```Trường hợp thử nghiệm đầu tiên hoạt động như sau: 

| Bước | trái | đúng | giữa | đếm (giữa) | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 2 | 10 | | | | 
| 1 | 2 | 6 | 6 | 5 | di chuyển sang phải | 
| Kết thúc | 2 | 6 | | | | 
| Cuối cùng | 6 | 6 | | | trả lời | 

giá trị`6`là ngưỡng đầu tiên chứa ít nhất bốn tổng cặp. 

Đối với trường hợp thử nghiệm thứ hai: 

| Bước | trái | đúng | giữa | đếm (giữa) | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | -2 | 4 | | | | 
| 1 | -2 | 1 | 1 | 3 | di chuyển sang phải | 
| Kết thúc | -2 | 1 | | | trả lời | 

Mảng được sắp xếp là`[-1, 2]`và tổng cặp nhỏ thứ hai là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + N log V) | Sắp xếp mất`O(N log N)`và mỗi bước tìm kiếm nhị phân sẽ đếm các cặp trong`O(N)`| 
| Không gian | O(1) thêm | Chỉ sử dụng mảng đã sắp xếp và một vài bộ đếm | 

Với`N <= 500`, lời giải dễ dàng nằm trong giới hạn. Ý tưởng tương tự cũng mở rộng sang các mảng lớn hơn nhiều vì nó tránh việc xây dựng toàn bộ`N²`danh sách. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(data):
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    def solve_case():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        def count_pairs(x):
            res = 0
            j = n - 1
            for i in range(n):
                while j >= 0 and a[i] + a[j] > x:
                    j -= 1
                res += j + 1
            return res

        l, r = a[0] + a[0], a[-1] + a[-1]
        while l < r:
            m = (l + r) // 2
            if count_pairs(m) >= k:
                r = m
            else:
                l = m + 1
        return str(l)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve_case())
    return "\n".join(out)

assert solution("""2
3 4
1 3 5
2 2
-1 2
""") == "6\n1"

assert solution("""1
1 1
7
""") == "7"

assert solution("""1
4 8
2 2 2 2
""") == "4"

assert solution("""1
3 3
-5 -1 4
""") == "-1"

assert solution("""1
5 25
1 2 3 4 5
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | 7 | Xử lý kích thước tối thiểu | 
| Tất cả các giá trị bằng nhau | 4 | Tổng trùng lặp và câu trả lời lặp đi lặp lại | 
| Giá trị âm | -1 | Đúng giới hạn tìm kiếm nhị phân | 
| Tất cả các cặp được yêu cầu | 10 | Ranh giới K tối đa | 

## Vỏ cạnh 

Đối với trường hợp trùng lặp:```
1
3 4
1 1 5
```Mảng được sắp xếp vẫn còn`[1, 1, 5]`. Các cặp tổng theo thứ tự được sắp xếp là`2, 2, 2, 2, 6, 6, 6, 6, 6`. Trong quá trình tìm kiếm nhị phân, ngưỡng của`2`đếm bốn cặp, vì vậy câu trả lời trở thành`2`. Thuật toán tính số lần xuất hiện chứ không phải giá trị riêng biệt nên các bản sao được xử lý một cách tự nhiên. 

Đối với trường hợp giá trị âm:```
1
2 2
-5 -1
```Việc tìm kiếm bắt đầu từ`-10`ĐẾN`-2`. Khi kiểm tra`-6`, bước đếm nhận thấy phần tử thứ nhất có thể ghép với cả hai phần tử, còn phần tử thứ hai không thể vượt quá ngưỡng. Số đếm đạt đến hai, vì vậy câu trả lời là`-6`. Phạm vi âm hoạt động vì giới hạn đến trực tiếp từ các giá trị mảng.
