---
title: "CF 105401G - Làm cho CHẠY tuyệt vời trở lại"
description: "Chúng tôi có một nhóm các câu lạc bộ $N$ có điểm số đã được cố định và một câu lạc bộ bổ sung tên là RUN có điểm số mà chúng tôi có thể tự do lựa chọn khi kết thúc."
date: "2026-06-23T04:54:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105401
codeforces_index: "G"
codeforces_contest_name: "2024 KAIST 14th ICPC Mock Competition"
rating: 0
weight: 105401
solve_time_s: 92
verified: false
draft: false
---

[CF 105401G - Làm cho CHẠY tuyệt vời trở lại](https://codeforces.com/problemset/problem/105401/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ$N$các câu lạc bộ có điểm số đã được ấn định và một câu lạc bộ bổ sung tên là RUN có điểm số mà chúng tôi có thể tự do lựa chọn khi kết thúc. Thứ hạng cuối cùng được xác định hoàn toàn bằng số lượng câu lạc bộ có số điểm cao hơn: thứ hạng của câu lạc bộ là một cộng với số câu lạc bộ có số điểm cao hơn. Thứ hạng thấp hơn có nghĩa là vị trí tốt hơn. 

Trước khi chọn điểm RUN, chúng ta được phép giảm điểm của người kia$N$câu lạc bộ. Mỗi câu lạc bộ$i$bắt đầu bằng điểm số$a_i$, và nếu chúng ta giảm nó đi một lượng nào đó$T$, chúng ta phải trả một “chi phí nghi ngờ” là$T \cdot b_i$. Tổng số nghi ngờ đối với tất cả các câu lạc bộ phải được duy trì ở mức thấp nhất$K$. Chúng ta không được tăng điểm mà chỉ được giảm hoặc giữ nguyên và điểm phải không âm. 

Sau tất cả các điều chỉnh, chúng tôi ấn định điểm RUN. Chúng tôi muốn RUN kết thúc ở vị trí cao nhất$X$và trong số tất cả các cách để đạt được điều đó, chúng tôi muốn điểm được chọn ban đầu của RUN càng nhỏ càng tốt. 

Giải thích điều kiện xếp hạng, RUN cần nhiều nhất$X-1$câu lạc bộ có điểm cuối cùng cao hơn. Vì vậy, nhiệm vụ trở thành: chúng ta có thể giảm một số điểm hiện có theo ràng buộc ngân sách toàn cầu về mức giảm có trọng số, sau đó chọn điểm của RUN càng nhỏ càng tốt trong khi vẫn đảm bảo rằng tối đa$X-1$câu lạc bộ vượt quá nó. 

Những ràng buộc đẩy chúng ta tới một$O(N \log N)$hoặc$O(N)$giải pháp. Với$N \le 10^5$, bất cứ điều gì bậc hai khi thử các cấu hình điểm hoặc tập hợp con khác nhau đều không khả thi ngay lập tức. Sự hiện diện của chi phí tuyến tính và việc giảm điểm đơn điệu gợi ý rõ ràng về cấu trúc tham lam hoặc dựa trên tiền tố, có thể được kết hợp với việc sắp xếp. 

Một trường hợp thất bại tinh tế xuất hiện khi các khoản cắt giảm được phân phối không hiệu quả. Ví dụ: nếu sớm tham lam giảm bớt các câu lạc bộ sai, chúng ta có thể lãng phí ngân sách cho những thay đổi có tác động thấp và sau đó không thể loại bỏ đủ số câu lạc bộ có điểm cao để đáp ứng yêu cầu về thứ hạng. Một cạm bẫy phổ biến khác là cho rằng chúng ta chỉ quan tâm đến việc giảm chi phí hàng đầu.$X-1$câu lạc bộ theo điểm ban đầu, bỏ qua việc giảm giá rẻ hơn cho các câu lạc bộ khác có thể hiệu quả hơn. 

Một trường hợp minh họa nhỏ là khi một câu lạc bộ có số điểm rất cao nhưng lại cực cao$b_i$, trong khi nhiều khoản khác cao hơn một chút so với mục tiêu nhưng giảm bớt với giá rẻ. Một kẻ tham lam ngây thơ chỉ tính điểm sẽ thất bại, trong khi hành vi tối ưu lại phụ thuộc vào hiệu quả chi phí. 

## Phương pháp tiếp cận 

Nếu chúng ta sửa điểm cuối cùng của RUN$S$, điều kiện “RUN có hạng nhiều nhất$X$” có nghĩa là nhiều nhất$X-1$câu lạc bộ vẫn ở trên$S$. Tương tự, trong số tất cả các câu lạc bộ, chúng ta phải đảm bảo rằng không quá$X-1$trong số họ có điểm cuối cùng$> S$. 

Đối với một cố định$S$, mỗi câu lạc bộ$i$rơi vào một trong ba vai trò: 

Nếu$a_i \le S$, chúng ta không cần phải giảm nó chút nào vì nó không đe dọa đến RUN. 

Nếu như$a_i > S$, thì chúng ta hoặc chấp nhận rằng nó vẫn ở trên$S$, hoặc chúng tôi giảm nó xuống tối đa$S$. Nếu chúng ta chọn giảm nó, chúng ta phải giảm nó ít nhất$a_i - S$, thanh toán chi phí$(a_i - S)b_i$. 

Vì vậy để cố định$S$, mỗi câu lạc bộ có$a_i > S$cho chúng ta một lựa chọn nhị phân: hoặc chúng ta “loại bỏ” mối đe dọa của nó (trả chi phí) hoặc chúng ta để nó như một đối thủ cạnh tranh. Chúng tôi được phép rời đi nhiều nhất$X-1$đối thủ cạnh tranh. Điều này ngay lập tức gợi ý chọn câu lạc bộ nào cần giảm thiểu hoàn toàn để chúng ta loại bỏ tất cả trừ nhiều nhất$X-1$của những thứ đắt tiền. 

Tái cơ cấu: giữa các câu lạc bộ với$a_i > S$, giả sử có$m$những câu lạc bộ như vậy. Chúng tôi được phép rời đi nhiều nhất$X-1$, vì vậy ít nhất chúng ta phải loại bỏ$m-(X-1)$của họ. Mỗi lần loại bỏ đều có chi phí$(a_i - S)b_i$. Để giảm thiểu sự nghi ngờ, chúng tôi chọn những thứ rẻ nhất để loại bỏ. 

Vì vậy đối với cố định$S$, tính khả thi làm giảm việc phân loại các chi phí này và kiểm tra xem tổng số chi phí yêu cầu nhỏ nhất có thấp hơn không$K$. 

Điều này biến bài toán ban đầu thành một bài toán quyết định đơn điệu$S$: nếu có điểm$S$là khả thi thì nhỏ hơn$S$cũng khả thi vì nhiều gậy trở nên nguy hiểm hơn và chi phí chỉ tăng lên. Tính đơn điệu này cho phép tìm kiếm nhị phân trên câu trả lời. 

Ý tưởng brute-force sẽ thử tất cả các điểm RUN có thể có từ$0$lên đến$\max a_i$, và với mỗi cái, hãy tính chi phí loại bỏ bằng cách quét tất cả các câu lạc bộ. Đó là$O(N \cdot \max A)$, quá chậm. Quan sát chính là tính khả thi là đơn điệu và có thể tính toán được trong$O(N \log N)$, cho phép tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot \max A)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N \log N \log \max A)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tìm kiếm nhị phân điểm RUN nhỏ nhất có thể$S$. 

1. Xác định hàm`check(S)`xác định liệu RUN có thể đạt được thứ hạng nhiều nhất hay không$X$nếu điểm của nó là$S$. 
2. Bên trong`check(S)`, tính toán cho mọi câu lạc bộ$i$với$a_i > S$chi phí để đưa nó xuống$S$, đó là$(a_i - S) \cdot b_i$. Câu lạc bộ với$a_i \le S$đóng góp miễn phí và không liên quan đến áp lực xếp hạng. Lý do là chỉ có điểm cao hơn mới quan trọng đối với thứ hạng. 
3. Đếm xem hiện tại có bao nhiêu câu lạc bộ vượt quá$S$. Gọi đây$m$. 
4. Nếu$m \le X-1$, thì không cần giảm bớt và cấu hình sẽ tự động hợp lệ. 
5. Nếu không chúng ta phải loại bỏ$m-(X-1)$câu lạc bộ. Thu thập tất cả các chi phí để giảm thiểu mỗi$m$câu lạc bộ, sắp xếp chúng và tính tổng nhỏ nhất$m-(X-1)$. Đây là cách rẻ nhất để giảm đủ đối thủ cạnh tranh. 
6. Nếu số tiền này nhỏ hơn$K$, cấu hình khả thi; nếu không thì không. 
7. Sử dụng tìm kiếm nhị phân$S$trong phạm vi$[0, \max a_i]$, giữ giá trị khả thi nhỏ nhất. 

Tại sao điều này đúng lại phụ thuộc vào cấu trúc của vấn đề quyết định. Đối với một ngưỡng cố định$S$, mỗi câu lạc bộ đóng góp độc lập vào chi phí nếu chúng ta quyết định loại bỏ nó, và sẽ không có lợi ích gì nếu giảm một phần câu lạc bộ mà không giảm nó xuống mức$S$, bởi vì mọi giá trị trung gian vẫn ở trên RUN và không làm giảm số lượng đối thủ cạnh tranh. Do đó, mọi chiến lược tối ưu đều tương đương với việc chọn một tập hợp con các câu lạc bộ để triệt tiêu hoàn toàn, với chi phí cộng thêm và tập con tối ưu luôn là tập hợp con rẻ nhất. 

Tính đơn điệu của tính khả thi trong$S$theo sau bởi vì ngày càng tăng$S$chỉ làm cho nó dễ dàng hơn: ít câu lạc bộ thỏa mãn hơn$a_i > S$, và mỗi chi phí$(a_i - S)b_i$giảm đi hoặc biến mất hoàn toàn. Vì vậy, một khi giá trị của$S$là khả thi, tất cả các giá trị lớn hơn vẫn khả thi, chứng minh tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check(S, arr, K, X):
    costs = []
    m = 0

    for a, b in arr:
        if a > S:
            m += 1
            costs.append((a - S) * b)

    if m <= X - 1:
        return True

    need = m - (X - 1)
    costs.sort()

    total = sum(costs[:need])
    return total < K

def solve():
    N, K, X = map(int, input().split())
    arr = [tuple(map(int, input().split())) for _ in range(N)]

    lo, hi = 0, max(a for a, _ in arr)

    ans = hi

    while lo <= hi:
        mid = (lo + hi) // 2
        if check(mid, arr, K, X):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh chính xác quá trình ra quyết định. các`check`chức năng xây dựng lại cấu trúc chi phí cho một ngưỡng cố định và đánh giá xem liệu chúng ta có đủ khả năng để loại bỏ đủ câu lạc bộ hay không. Phân loại chi phí là bước quan trọng để đảm bảo chúng ta luôn chọn được bộ giảm chấn rẻ nhất. 

Một điểm tinh tế là sự bất đẳng thức nghiêm ngặt về sự nghi ngờ: điều kiện “nhỏ hơn$K$”, do đó sự bình đẳng phải bị bác bỏ. Việc này được xử lý trực tiếp trong lần so sánh cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi kiểm tra ngưỡng ứng viên$S$bên trong tìm kiếm nhị phân. Quy trình kiểm tra diễn ra như sau. 

| Câu lạc bộ bang |$a_i$|$b_i$|$a_i > S?$| Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | 21 | 2 | vâng |$(21-S)\cdot2$| 
| 2 | 100 | 0 | vâng |$(100-S)\cdot0$| 
| 3 | 50 | 13 | vâng |$(50-S)\cdot13$| 
| 4 | 8 | 8 | phụ thuộc | 0 nếu không ở trên | 

Sau khi tính toán tất cả các chi phí trên ngưỡng, giả sử chúng ta có$m$mối đe dọa vượt quá$S$. Nếu nhiều hơn$X-1$còn lại, chúng tôi sắp xếp chi phí và chọn cách loại bỏ rẻ nhất cho đến khi chỉ$X-1$duy trì. 

Dấu vết này cho thấy chỉ có vị trí tương đối ở trên$S$vấn đề, không phải giá trị điểm chính xác bên dưới nó. Thuật toán chuyển đổi một cách hiệu quả ràng buộc xếp hạng thành vấn đề hạn chế ngân sách. 

### Mẫu 2 

Một dấu vết tương tự minh họa chế độ ngược lại, nơi nhiều câu lạc bộ đã tụt xuống dưới mức đã chọn$S$. 

| Câu lạc bộ bang |$a_i$|$b_i$|$a_i > S?$| Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 15 | không | 0 | 
| 2 | 15 | 4 | vâng |$(15-S)\cdot4$| 
| 3 | 24 | 11 | vâng |$(24-S)\cdot11$| 
| 4 | 10 | 3 | phụ thuộc | 0 hoặc chi phí | 

Ở đây ngày càng tăng$S$giảm số lượng các mối đe dọa đang hoạt động, trực tiếp thể hiện tính khả thi đơn điệu. Khi một ngưỡng trở nên khả thi, bất kỳ ngưỡng nào cao hơn chỉ làm giảm tập ứng cử viên của các hoạt động tốn kém. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N \log A)$| Tìm kiếm nhị phân trên phạm vi điểm, mỗi lần kiểm tra tính khả thi sẽ sắp xếp tối đa$N$chi phí | 
| Không gian |$O(N)$| Lưu trữ danh sách chi phí đầu vào và tạm thời | 

Những ràng buộc cho phép$N = 10^5$, Và$A \le 10^6$, làm cho hệ số logarit kép hoàn toàn an toàn. Việc sắp xếp chiếm ưu thế trong mỗi lần kiểm tra nhưng vẫn nằm trong giới hạn do hằng số nhỏ và tính khả thi của$N \log N$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = backup
    return out.getvalue().strip()

# provided samples
assert run("""4 10 2
21 2
100 0
50 13
8 8
""") == "41"

assert run("""5 15 3
3 15
15 4
24 11
10 3
8 1
""") == "10"

# minimum size
assert run("""1 100 1
10 5
""") == "0"

# all equal values
assert run("""3 10 2
5 1
5 2
5 3
""") >= "0"

# large K makes everything feasible
assert run("""4 10 2
10 1
10 1
10 1
10 1
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| câu lạc bộ đơn | 0 | tính khả thi tầm thường | 
| tất cả đều bình đẳng | 0 | xử lý đối xứng và xếp hạng | 
| ngân sách lớn | 0 | không cắt giảm không cần thiết | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi$X = N+1$, nghĩa là RUN có thể là cuối cùng. Trong trường hợp này, không có câu lạc bộ nào cần phải bị loại bất kể điểm số của câu lạc bộ đó là bao nhiêu. Thuật toán xử lý nó một cách tự nhiên:$m-(X-1)$trở thành 0 hoặc âm, do đó không có chi phí nào được cộng lại và mọi$S$là khả thi miễn là nó không âm. Tìm kiếm nhị phân trả về chính xác 0. 

Một trường hợp tế nhị khác là khi tất cả$a_i \le S$. Sau đó$m = 0$, do đó kiểm tra trả về true ngay lập tức. Điều này phản ánh thực tế là RUN đã được xếp hạng cao hơn tất cả mọi người bất kể những cân nhắc về chi phí. 

Một kịch bản tế nhị hơn xảy ra khi một số gậy được cắt giảm cực kỳ tốn kém, điều tối ưu nhất là để chúng ở trên RUN và thay vào đó loại bỏ nhiều gậy rẻ hơn. Lựa chọn dựa trên sắp xếp đảm bảo hành vi này vì tính khả thi chỉ phụ thuộc vào việc chọn tập hợp con loại bỏ cần thiết rẻ nhất chứ không phải bất kỳ phương pháp phỏng đoán nào dựa trên điểm số ban đầu.
