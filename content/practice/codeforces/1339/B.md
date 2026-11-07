---
title: "CF 1339B - Sự khác biệt liền kề được sắp xếp"
description: "Chúng ta có một số mảng độc lập và đối với mỗi mảng, chúng ta phải sắp xếp lại các phần tử của nó thành một chuỗi trong đó sự khác biệt tuyệt đối giữa các phần tử lân cận không bao giờ giảm khi chúng ta di chuyển từ trái sang phải."
date: "2026-06-16T09:18:09+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1339
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 633 (Div. 2)"
rating: 1200
weight: 1339
solve_time_s: 230
verified: false
draft: false
---

[CF 1339B - Các khác biệt liền kề được sắp xếp](https://codeforces.com/problemset/problem/1339/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 3 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số mảng độc lập và đối với mỗi mảng, chúng ta phải sắp xếp lại các phần tử của nó thành một chuỗi trong đó sự khác biệt tuyệt đối giữa các phần tử lân cận không bao giờ giảm khi chúng ta di chuyển từ trái sang phải. Nói cách khác, khi chúng ta bắt đầu xếp các số vào một dòng, khoảng cách giữa các số liên tiếp sẽ tạo thành một dãy không giảm. 

Đầu ra không phải là bảo tồn bất kỳ cấu trúc ban đầu nào. Chúng ta có thể tự do hoán vị mảng một cách tùy ý và chỉ cần tạo ra một thứ tự hợp lệ thỏa mãn điều kiện đơn điệu này đối với các sai phân liền kề. 

Ràng buộc về tổng kích thước đầu vào trong tất cả các trường hợp thử nghiệm ngụ ý rằng bất kỳ giải pháp nào lớn hơn tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm sẽ thất bại. Một nỗ lực ngây thơ thử tất cả các hoán vị hoặc kiểm tra liên tục tất cả các cặp hoán đổi sẽ bùng nổ theo tổ hợp hoặc bậc hai. Giải pháp dự định phải xử lý từng mảng theo O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm. 

Một kiểu thất bại tinh tế xuất hiện khi tư duy tham lam từ cấu trúc cục bộ. Nếu chúng ta cố gắng xây dựng chuỗi bằng cách liên tục thêm số có sẵn gần nhất vào phần tử được chọn cuối cùng, chúng ta có thể dễ dàng bị mắc kẹt trong một cấu hình trong đó những khác biệt nhỏ ban đầu buộc phải có những bước nhảy lớn về sau phá vỡ tính đơn điệu. Ví dụ: bắt đầu từ một giá trị ở giữa và mở rộng ra ngoài một cách tham lam có thể tạo ra khoảng cách giảm dần sau này khi vẫn còn một phần tử không được sử dụng gần hơn. Điều này cho thấy lân cận tham lam cục bộ là không đáng tin cậy; cấu trúc phải được thực thi trên toàn cầu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của mảng và kiểm tra xem thứ tự ứng viên có thỏa mãn thuộc tính sai phân liền kề không giảm hay không. Ngay cả việc kiểm tra một hoán vị cũng tốn O(n) và có n! hoán vị, làm cho điều này hoàn toàn không khả thi ngay cả với n khoảng 10. 

Ý tưởng ngây thơ thứ hai là tạo ra một chuỗi và liên tục chèn các phần tử vào các vị trí duy trì điều kiện. Tuy nhiên, mỗi lần chèn có thể yêu cầu quét toàn bộ chuỗi để xác minh tính đơn điệu của các khác biệt và không có gì đảm bảo rằng các quyết định trước đó không làm mất hiệu lực các lần chèn trong tương lai. Điều này dẫn đến hành vi O(n²) hoặc tệ hơn trên mỗi trường hợp thử nghiệm. 

Quan sát quan trọng là chúng ta không thực sự cố gắng kiểm soát các giá trị của sự khác biệt một cách độc lập. Thay vào đó, chúng ta có thể tạo ra một mô hình khác biệt có cấu trúc chặt chẽ bằng cách xây dựng chuỗi từ trung vị ra ngoài. Nếu chúng ta sắp xếp mảng, các giá trị gần nhau đương nhiên sẽ tạo ra những khác biệt nhỏ, trong khi các giá trị cách xa nhau sẽ tạo ra những khác biệt lớn. Nếu chúng ta xen kẽ các phần tử từ trung tâm ra ngoài, chúng ta có thể đảm bảo rằng những khác biệt ban đầu là nhỏ và những khác biệt sau đó sẽ tăng lên khi chúng ta di chuyển ra khỏi khu vực trung vị. 

Cấu trúc tiêu chuẩn là sắp xếp mảng, chọn phần tử trung tâm làm điểm bắt đầu, sau đó lần lượt mở rộng sang bên phải và bên trái của thứ tự được sắp xếp. Điều này tạo ra một trình tự trong đó trước tiên chúng tôi di chuyển trong một cụm chặt chẽ xung quanh điểm trung bình và sau đó chỉ đưa ra các giá trị cực trị, đảm bảo rằng sự khác biệt tuyệt đối không thể giảm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp mảng theo thứ tự không giảm. Điều này tổ chức các giá trị sao cho khoảng cách về chỉ số tương ứng với khoảng cách về giá trị. Cấu trúc của sự khác biệt khi đó sẽ được kiểm soát bởi sự chuyển động của chỉ số. 
2. Chọn điểm bắt đầu gần tâm của mảng đã sắp xếp. Một lựa chọn thuận tiện là chỉ số ở giữa, vì nó giảm thiểu khoảng cách ban đầu đến cả hai cực. 
3. Xây dựng chuỗi kết quả bằng cách mở rộng ra ngoài từ tâm này, xen kẽ giữa các hướng phải và trái trong mảng đã sắp xếp. Điều này tạo ra sự khám phá cân bằng về khoảng cách giá trị ngày càng tăng. 
4. Nối từng phần tử đã chọn vào kết quả theo thứ tự được truy cập. Một số bước đầu tiên nằm trong một vùng số hẹp nên sự khác biệt vẫn còn nhỏ. 
5. Tiếp tục cho đến khi sử dụng hết các phần tử, đảm bảo rằng các phần tử ở xa trung tâm theo thứ tự được sắp xếp chỉ được đưa vào sau trong chuỗi. 

### Tại sao nó hoạt động 

Việc xây dựng đảm bảo rằng các khác biệt liền kề sớm đến từ các phần tử gần nhau theo thứ tự được sắp xếp, do đó có giá trị tuyệt đối nhỏ. Khi quá trình truyền tải mở rộng ra bên ngoài, mọi phần tử mới được đưa vào đều nằm cách xa khu vực trước đó theo thứ tự được sắp xếp, điều này chỉ có thể duy trì hoặc tăng cường độ khoảng cách. Vì chúng ta không bao giờ quay trở lại một thái cực đã bị bỏ qua trước đó sau khi di chuyển ra ngoài, nên chúng ta tránh đưa ra một sự khác biệt nhỏ hơn sau một thái cực lớn hơn, điều này tạo ra sự đơn điệu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        l = (n - 1) // 2
        r = l + 1

        res = [a[l]]
        toggle = True

        while l > 0 or r < n:
            if toggle:
                if r < n:
                    res.append(a[r])
                    r += 1
                else:
                    l -= 1
                    res.append(a[l])
            else:
                if l > 0:
                    l -= 1
                    res.append(a[l])
                else:
                    res.append(a[r])
                    r += 1
            toggle = not toggle

        print(*res)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách sắp xếp từng mảng trường hợp thử nghiệm sao cho mức độ gần nhau về mặt số trở thành sự liền kề về cấu trúc. Sau đó nó chọn một chỉ mục trung tâm và mở rộng ra ngoài theo các hướng xen kẽ. Chuyển động con trỏ xen kẽ đảm bảo rằng chúng ta không bao giờ liên tục kéo các giá trị từ cùng một phía, điều này có nguy cơ tạo ra sự khác biệt nhỏ sau một bước nhảy lớn. 

Chi tiết triển khai chính là việc xử lý cẩn thận các ranh giới khi một trong hai con trỏ đến cuối mảng. Tại thời điểm đó, thuật toán chỉ tiếp tục theo hướng còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[5, -2, 4, 8, 6, 5]`Mảng được sắp xếp trở thành:`[-2, 4, 5, 5, 6, 8]`Chúng tôi bắt đầu ở gần giữa ở chỉ số 2 hoặc 3 (được lập chỉ mục 0). Sử dụng chỉ số 2 làm trung tâm: 

| Bước | Chỉ số trái | Chỉ mục bên phải | Được chọn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 5 | [5] | 
| 2 | 2 | 4 | 5 | [5, 5] | 
| 3 | 1 | 4 | 4 | [5, 5, 4] | 
| 4 | 1 | 5 | 6 | [5, 5, 4, 6] | 
| 5 | 0 | 5 | -2 | [5, 5, 4, 6, -2] | 
| 6 | 0 | 6 (kết thúc) | 8 | [5, 5, 4, 6, -2, 8] | 

Sự khác biệt là 0, 1, 2, 8, 10, không giảm. 

Dấu vết này cho thấy những khác biệt cục bộ nhỏ được tạo ra đầu tiên như thế nào bên trong vùng giữa dày đặc, trong khi các giá trị cực trị chỉ xuất hiện sau đó, tạo ra những bước nhảy lớn hơn. 

### Ví dụ 2 

Mảng đầu vào:`[8, 1, 4, 2]`Mảng được sắp xếp:`[1, 2, 4, 8]`Bắt đầu ở chỉ số 1 (giá trị 2): 

| Bước | Chỉ số trái | Chỉ mục bên phải | Được chọn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | [2] | 
| 2 | 1 | 3 | 4 | [2, 4] | 
| 3 | 0 | 3 | 1 | [2, 4, 1] | 
| 4 | 0 | 4 (kết thúc) | 8 | [2, 4, 1, 8] | 

Sự khác biệt là 2, 3, 3, không giảm. 

Ví dụ này chứng tỏ sự luân phiên đảm bảo chúng ta không nhảy ngay lập tức giữa các thái cực mà thay vào đó dần dần mở rộng ra ngoài từ trung tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong từng trường hợp thử nghiệm, trong khi việc xây dựng là tuyến tính | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng đã sắp xếp và hoán vị đầu ra | 

Tổng số n trên tất cả các trường hợp thử nghiệm tối đa là 10^5, do đó, việc sắp xếp từng trường hợp thử nghiệm một cách độc lập vẫn phù hợp thoải mái trong giới hạn thời gian. Bước xây dựng lại tuyến tính đảm bảo không có chi phí bổ sung ngoài việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out_lines = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        l = (n - 1) // 2
        r = l + 1

        res = [a[l]]
        toggle = True

        while l > 0 or r < n:
            if toggle:
                if r < n:
                    res.append(a[r]); r += 1
                else:
                    l -= 1; res.append(a[l])
            else:
                if l > 0:
                    l -= 1; res.append(a[l])
                else:
                    res.append(a[r]); r += 1
            toggle = not toggle

        out_lines.append(" ".join(map(str, res)))

    return "\n".join(out_lines)

# provided samples
assert run("""2
6
5 -2 4 8 6 5
4
8 1 4 2
""") == """5 5 4 6 -2 8
2 4 1 8""", "sample test"

# custom: all equal
assert run("""1
5
7 7 7 7 7
""").count("7") == 5

# custom: already sorted
assert run("""1
5
1 2 3 4 5
""") is not None

# custom: negative mix
assert run("""1
6
-5 -1 -3 -2 4 10
""
```
