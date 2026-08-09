---
title: "CF 102460I - Quang phổ"
description: "Chúng ta cần xây dựng lại một mảng số nguyên tăng dần (X) từ tất cả các khoảng cách theo cặp giữa các phần tử của nó. Phần tử đầu tiên được cố định ở mức 0 và mọi phần tử đều nằm trong khoảng từ 0 đến 999."
date: "2026-08-08T10:13:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102460
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ICPC Asia Taipei-Hsinchu Regional Contest"
rating: 0
weight: 102460
solve_time_s: 164
verified: true
draft: false
---

[CF 102460I - Quang phổ](https://codeforces.com/problemset/problem/102460/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng lại một mảng số nguyên tăng dần (X) từ tất cả các khoảng cách theo cặp giữa các phần tử của nó. Phần tử đầu tiên được cố định ở mức 0 và mọi phần tử nằm trong khoảng từ 0 đến 999. Đối với mỗi cặp vị trí (i<j), phổ chứa giá trị (x_j-x_i), bao gồm các giá trị lặp lại. Đầu vào cung cấp các khoảng cách (\frac{n(n-1)}2) này theo thứ tự được sắp xếp và nhiệm vụ là tìm mọi mảng có thể tạo ra chính xác nhiều tập hợp đó. 

Ví dụ: phổ (1,2,3) của (n=3) có thể đến từ (0,1,3) hoặc (0,2,3). Các mảng này khác nhau nhưng khoảng cách theo cặp của chúng giống nhau. Chúng ta phải xuất cả hai, theo thứ tự từ điển. 

Giới hạn trên (n\le62) có nghĩa là phổ chứa nhiều nhất (\frac{62\cdot61}{2}=1891) giá trị. Nó đủ nhỏ để chúng ta duy trì bảng tần số được lập chỉ mục theo khoảng cách. Giới hạn tọa độ của 999 thậm chí còn hữu ích hơn: mọi khoảng cách đều thuộc phạm vi nhỏ từ 1 đến 999, do đó, việc kiểm tra và định vị các khoảng cách còn lại có thể được thực hiện bằng một mảng có kích thước cố định thay vì cây cân bằng hoặc bảng băm. Bản thân việc tìm kiếm không thể là đa thức trong bài toán quay vòng tổng quát nên lời giải phải khai thác cấu trúc khoảng cách lớn nhất còn lại để làm cho việc phân nhánh trở nên cực kỳ nhỏ. 

Trường hợp không rõ ràng đầu tiên là (n=2). Chỉ có một khoảng cách. Đối với đầu vào`2`theo sau là`7`, câu trả lời duy nhất là`0 7`. Việc triển khai đệ quy giả định rằng có một điểm bên trong cần tái cấu trúc sẽ thất bại ở đây. 

Trường hợp thứ hai là phổ không hợp lệ có giá trị lớn nhất vượt quá giới hạn tọa độ. Ví dụ,```
2
1000
```không có câu trả lời hợp lệ vì mảng duy nhất có thể là`0 1000`, vi phạm giới hạn (x_i\le999). Việc triển khai bất cẩn có thể xây dựng lại các điểm cuối mà không kiểm tra ràng buộc tọa độ. 

Khoảng cách lặp đi lặp lại cũng cần được coi là nhiều tập hợp. Ví dụ,```
4
2 2 2 4 4 6
```có câu trả lời`0 2 4 6`. Giá trị 2 xảy ra ba lần vì ba cặp khác nhau cách nhau hai. Việc xử lý phổ như một tập hợp sẽ làm mất thông tin này và có thể chấp nhận các mảng không hợp lệ. 

Một trường hợp đặc biệt quan trọng là khi khoảng cách lớn nhất hiện chưa giải thích được xảy ra hai lần và bằng chính xác một nửa tổng chiều rộng. Vì```
5
1 1 1 1 2 2 2 3 3 4
```câu trả lời là`0 1 2 3 4`. Khoảng cách 2 xảy ra hai lần giữa điểm cuối và điểm giữa, nhưng chỉ có một tọa độ ở giữa, đó là 2. Một quy tắc chèn một cách mù quáng cả (d) và (W-d) sẽ chèn cùng một tọa độ hai lần. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp sẽ cố định các điểm cuối ở mức 0 và một số giá trị tối đa (W), sau đó chọn tọa độ (n-2) còn lại từ các số nguyên giữa chúng. Vì (W\le999), trường hợp xấu nhất xét 

[ 
\binom{998}{n-2} 
] 

các tập tọa độ bên trong khác nhau. Đối với mỗi mảng ứng cử viên, chúng tôi tạo ra tất cả khoảng cách (\frac{n(n-1)}2) và so sánh bội số của chúng với phổ đầu vào. Do đó, công việc trong trường hợp xấu nhất là 

[ 
O\left(\binom{998}{n-2}n^2\right). 
] 

Với (n=62), điều này vượt xa khả năng thực hiện. 

Lực lượng vũ phu hoạt động vì việc kiểm tra rõ ràng mọi ứng cử viên chắc chắn là đúng, nhưng nó hoàn toàn bỏ qua thông tin mạnh nhất có trong quang phổ: khoảng cách lớn nhất của nó. 

Gọi khoảng cách lớn nhất là (W). Bất kỳ mảng hợp lệ nào cũng phải chứa cả 0 và (W), vì khoảng cách lớn nhất chỉ có thể là khoảng cách giữa tọa độ tối thiểu và tối đa. Khi các điểm cuối đó đã được cố định, hãy xem xét khoảng cách lớn nhất (d) chưa được giải thích bằng các điểm đã chọn. 

Chỉ có hai tọa độ có thể giải thích khoảng cách này thông qua một điểm cuối. Một là (d), vì khoảng cách của nó tới 0 là (d). Cái còn lại là (W-d), vì khoảng cách của nó với (W) là (d). Do đó, mỗi lần hoàn thành hợp lệ phải chứa ít nhất một trong hai tọa độ này. 

Điều này mang lại chiến lược tái thiết đường quay vòng tiêu chuẩn. Chúng tôi liên tục lấy khoảng cách lớn nhất không giải thích được và thử các tọa độ duy nhất có thể giải thích được khoảng cách đó. Bất cứ khi nào một tọa độ được đề xuất, tất cả khoảng cách của nó đến tọa độ đã chọn phải tồn tại trong nhiều tập hợp còn lại. Nếu thiếu một khoảng cách cần thiết thì việc phân nhánh là không thể và có thể bị loại bỏ ngay lập tức. 

Việc quay lui cơ bản đó vẫn chưa đủ cho vấn đề này. Cấp số cộng có thể tạo ra nhiều nhánh có dạng đối xứng. Quan sát bổ sung là về tính đa dạng. 

Giả sử khoảng cách không giải thích được lớn nhất là (d) và bội số còn lại của nó lớn hơn hai. Trạng thái đó là không thể được. Ở giai đoạn này, sự xuất hiện không giải thích được của (d) chỉ có thể được giải thích bằng cách đặt (d) hoặc (W-d). Chỉ có hai lần xuất hiện do điểm cuối tạo ra. Bất kỳ cặp điểm không được đặt trong tương lai nào có hiệu là (d) cũng sẽ có khoảng cách lớn hơn đến điểm cuối và khoảng cách lớn hơn đó sẽ được xử lý trước tiên. Do đó, có thể giữ lại tối đa hai bản sao của khoảng cách lớn nhất hiện tại. 

Nếu (d) xảy ra chính xác hai lần và (d\ne W-d), cả (d) và (W-d) đều bị ép buộc. Không có lý do gì để phân nhánh. Nếu chỉ một trong số chúng được chèn vào thì chỉ một bản sao của (d) sẽ được giải thích bằng điểm cuối, trong khi bản sao còn lại sẽ phải đến từ một cặp trong tương lai có khoảng cách điểm cuối lớn hơn lẽ ra đã được xử lý. 

Nếu (d=W-d), hai ứng viên có cùng điểm. Đây là trường hợp điểm giữa, do đó một tọa độ duy nhất giải thích được cả hai khoảng cách điểm cuối. 

Các quy tắc bội số này thu gọn các nhánh có vấn đề tạo nên một DFS quay vòng ngây thơ theo cấp số nhân trong thực tế. Việc tìm kiếm kết quả vẫn theo cấp số nhân trong trường hợp xấu nhất về mặt lý thuyết, nhưng giới hạn tọa độ và quy tắc sắp xếp bắt buộc mạnh mẽ làm cho nó trở nên thiết thực đối với các giới hạn đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O\left(\binom{998}{n-2}n^2\right)) | (O(n^2)) | Quá chậm | 
| Quay lại với việc cắt tỉa khoảng cách lớn nhất | (O(2^n n^2 + Rn\log n)) trường hợp xấu nhất | (O(1000+n+Rn)) | Đã chấp nhận | 

Ở đây (R) là số nghiệm hợp lệ. Thuật ngữ (2^n) mô tả giới hạn cây tìm kiếm lý thuyết. Trong thực tế, quy tắc bội số sẽ loại bỏ các cây con đối xứng lớn khiến cho phiên bản đơn giản bị hết thời gian. 

## Hướng dẫn thuật toán

1. Đọc khoảng cách (\frac{n(n-1)}2) và tìm khoảng cách tối đa (W) của chúng. Nếu (W>999) thì không có mảng nào hợp lệ. Ngược lại, khởi tạo một mảng tần số`cnt`, Ở đâu`cnt[d]`lưu trữ bao nhiêu bản sao của khoảng cách (d) vẫn chưa được sử dụng. 
2. Đặt 0 và (W) vào tập tọa độ hiện tại. Khoảng cách (W) đã được giải thích bằng cặp này, do đó hãy giảm`cnt[W]`bởi một. Các điểm cuối này là bắt buộc vì không có cặp nào khác có thể có khoảng cách lớn hơn khoảng cách giữa tọa độ tối thiểu và tối đa. 
3. Tại mọi trạng thái đệ quy, tìm khoảng cách (d) lớn nhất có tần số còn lại là dương. Đây là khoảng cách tiếp theo phải được giải thích. 
4. Nếu tất cả (n) tọa độ đã được đặt, hãy ghi lại tập hợp hiện tại dưới dạng nghiệm. Mỗi lần chèn thành công sẽ loại bỏ chính xác khoảng cách do điểm mới đưa ra, do đó việc đạt tới (n) điểm có nghĩa là toàn bộ phổ đã được sử dụng. 
5. Nếu bội số còn lại của (d) lớn hơn 2 thì dừng nhánh này. Chỉ có hai vị trí điểm cuối có thể có, (d) và (W-d), có thể chiếm khoảng cách lớn nhất hiện tại. 
6. Nếu`cnt[d] == 2`và (d\ne W-d), hãy thử đặt cả (d) và (W-d) lại với nhau. Điều này là bắt buộc, vì vậy không có sự phân nhánh ở trạng thái này. 
7. Nếu không, hãy thử (d) làm tọa độ ứng viên. Đối với ứng cử viên đó, hãy tính khoảng cách của nó tới mọi tọa độ đã được đặt. Ứng cử viên chỉ hợp pháp nếu mỗi khoảng cách yêu cầu có đủ bội số còn lại. Nếu hợp pháp, hãy xóa những khoảng cách đó, thêm tọa độ, lặp lại và sau đó khôi phục mọi thứ trong khi quay lại. 
8. Nếu (d\ne W-d), cũng thử (W-d) theo cách tương tự. Đây là hai tọa độ duy nhất có khả năng giải thích khoảng cách lớn nhất hiện tại. 
9. Khi quá trình tìm kiếm kết thúc, hãy sắp xếp tất cả các mảng được xây dựng lại theo từ điển. Các tọa độ bên trong một giải pháp cũng được sắp xếp trước khi lưu trữ chúng vì biểu diễn đầu ra được yêu cầu ngày càng tăng. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`cnt`luôn biểu thị chính xác khoảng cách chưa được tạo bởi tọa độ hiện tại trong`points`. Ban đầu, chỉ có khoảng cách điểm cuối (W) bị xóa. Bất cứ khi nào một tọa độ mới được thêm vào, chính xác khoảng cách của nó đến tất cả các tọa độ hiện có sẽ bị xóa, do đó bất biến vẫn đúng. 

Đối với khoảng cách còn lại lớn nhất (d), một lần hoàn thành hợp lệ phải chứa (d) hoặc (W-d). Nếu cả hai điểm đều không hiện diện thì bất kỳ cặp điểm nào trong tương lai tạo ra khoảng cách (d) sẽ có khoảng cách điểm cuối lớn hơn (d), mâu thuẫn với thực tế rằng (d) hiện là khoảng cách lớn nhất không giải thích được. Do đó việc tìm kiếm không bao giờ loại bỏ một giải pháp hợp lệ. 

Việc cắt tỉa bội số tuân theo cùng một lập luận. Hai vị trí điểm cuối không thể giải thích được nhiều hơn hai bản sao không giải thích được của (d), do đó trạng thái đó không thể dẫn đến giải pháp. Khi còn lại chính xác hai bản sao và (d\ne W-d), cả hai vị trí điểm cuối đều bị ép buộc. Trường hợp điểm giữa thì khác vì một điểm tại (W/2) có khoảng cách (d) tới cả hai điểm cuối. 

Mọi ứng cử viên chỉ được chấp nhận sau khi tất cả các khoảng cách mới được tạo đã được tìm thấy trong nhiều tập hợp còn lại. Do đó, mọi mảng được ghi đều có chính xác phổ được yêu cầu. Vì mỗi lần hoàn thành hợp lệ đều tuân theo một trong các lựa chọn ứng cử viên được đệ quy xem xét nên cuối cùng mọi mảng hợp lệ đều được tìm thấy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, spec):
    m = n * (n - 1) // 2

    if len(spec) != m:
        return []

    width = spec[-1]

    if width > 999:
        return []

    cnt = [0] * (width + 1)

    for d in spec:
        if d <= 0 or d > width:
            return []
        cnt[d] += 1

    # The maximum distance must be between 0 and width.
    cnt[width] -= 1

    points = [0, width]
    answers = []

    def largest_remaining():
        for d in range(width, 0, -1):
            if cnt[d]:
                return d
        return 0

    def place(candidates):
        """Try placing all candidates as one forced/branched operation."""
        k = len(candidates)

        if len(points) + k > n:
            return

        if len(set(candidates)) != k:
            return

        for x in candidates:
            if x <= 0 or x >= width or x in points:
                return

        need = {}

        # Distances from every new point to every old point.
        for x in candidates:
            for y in points:
                d = abs(x - y)
                need[d] = need.get(d, 0) + 1

        # Distances between new points when two are inserted together.
        for i in range(k):
            for j in range(i + 1, k):
                d = abs(candidates[i] - candidates[j])
                need[d] = need.get(d, 0) + 1

        # Check whether the remaining spectrum contains all
        # distances introduced by the new points.
        for d, amount in need.items():
            if d == 0 or d > width or cnt[d] < amount:
                return

        # Apply the changes.
        for d, amount in need.items():
            cnt[d] -= amount

        points.extend(candidates)

        dfs()

        for _ in candidates:
            points.pop()

        for d, amount in need.items():
            cnt[d] += amount

    def dfs():
        if len(points) == n:
            answers.append(tuple(sorted(points)))
            return

        d = largest_remaining()

        if d == 0:
            return

        # At the current largest unexplained distance, at most
        # two copies can still be possible.
        if cnt[d] > 2:
            return

        reflected = width - d

        # Two copies force both symmetric positions, except when
        # they coincide at the midpoint.
        if cnt[d] == 2 and d != reflected:
            place((d, reflected))
            return

        # One copy means either endpoint-side candidate may be used.
        # When d == reflected there is only one distinct candidate.
        place((d,))

        if reflected != d:
            place((reflected,))

    dfs()

    answers.sort()
    return answers

def main():
    n_line = input().strip()
    if not n_line:
        return

    n = int(n_line)
    m = n * (n - 1) // 2

    spec = []
    while len(spec) < m:
        spec.extend(map(int, input().split()))

    answers = solve_case(n, spec)

    out = [str(len(answers))]
    for ans in answers:
        out.append(" ".join(map(str, ans)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Mảng tần số được lập chỉ mục trực tiếp theo khoảng cách. Điều này tốt hơn một từ điển ở đây vì khoảng cách lớn nhất có thể chỉ là 999, vì vậy mỗi lần tra cứu và cập nhật là thời gian không đổi với các hằng số rất nhỏ. 

Việc khởi tạo sẽ cố định hai điểm cuối và xóa khoảng cách của chúng khỏi`cnt`. Điều này xử lý trường hợp (n=2) một cách tự nhiên, vì sau khi loại bỏ khoảng cách duy nhất, đệ quy ngay lập tức thấy rằng cả hai điểm yêu cầu đều đã có mặt. 

các`place`chức năng thực hiện hoạt động multiset trung tâm. Đầu tiên nó từ chối các tọa độ nằm ngoài khoảng mở ((0,W)), vì các điểm cuối đã chiếm 0 và (W). Nó cũng từ chối các tọa độ trùng lặp vì mảng ban đầu phải chứa các giá trị riêng biệt. 

các`need`từ điển là cần thiết vì một số cặp mới được tạo có thể có cùng khoảng cách. Ví dụ: chèn điểm giữa vào một khoảng có chiều rộng đều sẽ tạo ra khoảng cách như nhau đến cả hai điểm cuối. Tổng hợp các bội số trước khi sửa đổi`cnt`ngăn chặn một chuỗi kiểm tra riêng lẻ vô tình chấp nhận một tập hợp nhiều bản sao không đủ. 

Trường hợp hai ứng cử viên được xử lý bằng cách chuyển cả hai tọa độ tới`place`cùng một lúc. Khoảng cách lẫn nhau của họ được bao gồm trong`need`, đó là một chi tiết tinh tế khác. Việc bỏ qua khoảng cách đó sẽ để lại một bản sao không giải thích được trong phổ và có thể chấp nhận việc tái tạo một cách không chính xác. 

Độ sâu đệ quy tối đa là 62, vì vậy giới hạn đệ quy của Python không phải là vấn đề đáng lo ngại. Các số nguyên Python cũng có độ chính xác tùy ý, mặc dù mọi giá trị liên quan trong bài toán này nhiều nhất là 999 và không cần số học lớn. 

Cuối cùng, các câu trả lời được sắp xếp sau khi xây dựng lại. Thứ tự DFS được xác định bởi bội số phổ chứ không phải thứ tự từ điển, do đó việc dựa vào thứ tự tìm kiếm sẽ không đáp ứng được yêu cầu đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4
2 2 2 4 4 6
```Các chuyển đổi trạng thái quan trọng là: 

| Bước | Khoảng cách còn lại lớn nhất | Đếm | Hành động của ứng viên | Điểm hiện tại | Phổ còn lại | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 6 | 1 | Sửa điểm cuối 0 và 6 | 0, 6 | 2,2,2,4,4 | 
| 1 | 4 | 2 | Lực lượng 2 và 4 | 0,2,4,6 | 2 | 
| 2 | 2 | 1 | Không cần điểm mới vì 2 đã được sử dụng làm khoảng cách giữa 2 và 4 | 0,2,4,6 | trống | 

Khoảng cách 4 xảy ra hai lần nên tọa độ 4 và (6-4=2) là bắt buộc. Việc chèn chúng lại với nhau cũng tạo ra khoảng cách tương hỗ 2, tiêu tốn bản sao cuối cùng của 2. Quá trình xây dựng lại đã hoàn tất. 

Kết quả đầu ra là```
1
0 2 4 6
```Ví dụ này thực hiện quy tắc nhân hai và nhu cầu đếm khoảng cách giữa hai tọa độ được chèn đồng thời. 

### Mẫu 2 

Phổ đầu vào là```
3 3 6 9 9 12 12 15 18 21
```Việc xây dựng lại bắt đầu với điểm cuối 0 và 21. 

| Bước | Lớn nhất còn lại | Đếm | Ứng viên đã thử | Khoảng cách mới cần thiết | Điểm hiện tại | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 21 | 1 | 0, 21 cố định | 21 | 0,21 | 
| 1A | 18 | 1 | 18 | 18,3 | 0,18,21 | 
| 2A | 15 | 1 | 15 | 15,3,6 | bị từ chối vì không có 3 | 
| 2B | 15 | 1 | 6 | 6,15,12 | 0,6,18,21 | 
| 3A | 12 | 1 | 12 | 12,9,6 | bị từ chối vì không có 6 | 
| 3B | 12 | 1 | 9 | 9,12,3 | 0,6,9,18,21 | 
| Cuối cùng | 0 | 0 | hoàn thành | không | 0,6,9,18,21 | 
| 1C | 18 | 1 | 3 | 3,18 | 0,3,21 | 
| 2C | 15 | 1 | 15 | 15,6,12 | 0,3,15,21 | 
| 3C | 12 | 1 | 12 | 12,9,9,3 | 0,3,12,15,21 | 
| Cuối cùng | 0 | 0 | hoàn thành | không | 0,3,12,15,21 | 

Cả hai nhánh đều hợp lệ, tạo ra```
2
0 3 12 15 21
0 6 9 18 21
```Dấu vết chứng minh tại sao một ứng cử viên không được chấp nhận chỉ vì tọa độ của nó hợp lý. Mọi khoảng cách từ tọa độ mới đến mọi tọa độ hiện có đều phải có sẵn bội số chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(2^n n^2 + Rn\log n)) trường hợp xấu nhất | Tối đa hai nhánh ứng cử viên tồn tại ở trạng thái bình thường, mỗi nhánh ứng cử viên sẽ kiểm tra khoảng cách tới tối đa (n) điểm hiện có. Tìm khoảng cách quét còn lại lớn nhất ở tối đa 999 mục. | 
| Không gian | (O(1000+n+Rn)) | Mảng tần số sử dụng (O(1000)), đệ quy và sử dụng tập tọa độ hiện tại (O(n)) và lưu trữ tất cả các câu trả lời yêu cầu (O(Rn)). | 

Việc tìm kiếm lý thuyết vẫn còn theo cấp số nhân vì bài toán xây dựng lại đường quay vòng nói chung không được biết là có thể chấp nhận một giải pháp thời gian đa thức. Cải tiến thực tế có liên quan là quy tắc bội số khoảng cách lớn nhất thường xuyên biến việc phân nhánh hai chiều thành việc chèn bắt buộc hai tọa độ. Giới hạn tọa độ 999 cũng khiến cho các thao tác tần số trở nên cực kỳ rẻ. Với tối đa 1891 khoảng cách đầu vào và độ sâu đệ quy tối đa là 62, điều này rất phù hợp với giới hạn 5 giây và 1024 MB đã nêu. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp biên tập được lưu dưới dạng`solution.py`, với`solve_case`có sẵn từ nó.```python
from solution import solve_case

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    n = data[0]
    spec = data[1:]

    ans = solve_case(n, spec)

    out = [str(len(ans))]
    for x in ans:
        out.append(" ".join(map(str, x)))

    return "\n".join(out) + "\n"

# Provided sample 1
assert run("""
4
2 2 2 4 4 6
""") == """1
0 2 4 6
""", "sample 1"

# Provided sample 2
assert run("""
5
3 3 6 9 9 12 12 15 18 21
""") == """2
0 3 12 15 21
0 6 9 18 21
""", "sample 2"

# Provided sample 3
assert run("""
5
6 7 8 9 10
""") == """0
""", "sample 3"

# Minimum-size input, n = 2.
assert run("""
2
7
""") == """1
0 7
""", "minimum n"

# All spectrum values are equal, possible only for n = 2.
assert run("""
2
999
""") == """1
0 999
""", "maximum coordinate"

# Two different solutions, catches reflection handling.
assert run("""
3
1 2 3
""") == """2
0 1 3
0 2 3
""", "two reflected solutions"

# Invalid repeated distances for n = 3.
assert run("""
3
1 1 1
""") == """0
""", "impossible spectrum"

# Largest distance outside the allowed coordinate range.
assert run("""
2
1000
""") == """0
""", "coordinate boundary"

# Maximum n. Construct X = 0, 1, ..., 61.
x = list(range(62))
spec = []
for i in range(62):
    for j in range(i + 1, 62):
        spec.append(j - i)
spec.sort()

max_input = "62\n" + " ".join(map(str, spec)) + "\n"
max_expected = "1\n" + " ".join(map(str, x)) + "\n"

assert run(max_input) == max_expected, "maximum n"
```Các trường hợp tùy chỉnh có thể được tóm tắt như sau. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 7`|`1 / 0 7`| Tối thiểu (n), khởi tạo điểm cuối | 
|`2 / 999`|`1 / 0 999`| Ranh giới tọa độ tối đa | 
|`3 / 1 2 3`| Hai giải pháp | Phản ánh và sắp xếp từ điển | 
|`3 / 1 1 1`|`0`| bội số không hợp lệ | 
|`2 / 1000`|`0`| Từ chối chiều rộng không thể | 
|`62`với phổ của`0..61`| Một giải pháp | Tối đa (n), độ sâu đệ quy, phổ lớn | 

## Vỏ cạnh 

Với (n=2), đầu vào```
2
7
```bắt đầu với điểm cuối 0 và 7. Giá trị phổ duy nhất được cặp của chúng sử dụng ngay lập tức, do đó tập hợp hiện tại đã có hai phần tử được yêu cầu. Bản ghi thuật toán`0 7`mà không cần cố gắng chèn tọa độ bên trong. 

Đối với ranh giới tọa độ,```
2
999
```chiều rộng chính xác là 999, vì vậy nó được chấp nhận. Kết quả là`0 999`. Việc kiểm tra việc thực hiện`width > 999`, còn hơn là`width >= 999`, điều này tránh được lỗi từng cái một ở ranh giới trên hợp pháp. 

Đối với chiều rộng ngoài phạm vi,```
2
1000
```bản thân khoảng cách tối đa sẽ yêu cầu một phần tử ở mức 1000 vì phần tử đầu tiên được cố định ở mức 0. Thuật toán sẽ loại bỏ trường hợp đó trước khi bắt đầu tìm kiếm. 

Đối với những khoảng cách lặp đi lặp lại,```
4
2 2 2 4 4 6
```chiều rộng 6 sửa các điểm cuối. Khoảng cách lớn nhất tiếp theo là 4 với bội số là 2, nên cả 4 và (6-4=2) đều là bắt buộc. Khoảng cách lẫn nhau của chúng là 2 khác, tiêu thụ cả ba bản sao của khoảng cách 2. Mảng cuối cùng là`0 2 4 6`. 

Đối với trường hợp trung điểm,```
5
1 1 1 1 2 2 2 3 3 4
```chiều rộng là 4. Khi khoảng cách 2 trở thành khoảng cách còn lại lớn nhất với bội số 2, thì cả hai ứng viên đều có tọa độ 2 vì (4-2=2). Thuật toán nhận ra các ứng viên trùng khớp và chỉ chèn một điểm. Điểm đó tạo ra hai khoảng cách điểm cuối là 2, trong khi khoảng cách của nó đến tọa độ đã chọn khác chiếm các bản sao còn lại. 

Đối với phổ không hợp lệ,```
3
1 1 1
```chiều rộng là 1, do đó các điểm cuối sẽ phải là 0 và 1. Tọa độ riêng biệt thứ ba không thể khớp hoàn toàn giữa chúng. Tìm kiếm đệ quy không tìm thấy ứng cử viên hợp pháp nào và trả về kết quả bằng không. 

Đối với trường hợp có hai nghiệm,```
3
1 2 3
```điểm cuối là 0 và 3. Khoảng cách lớn nhất còn lại là 2, có ứng cử viên là 2 và 1. Đặt 1 tạo ra`0 1 3`, trong khi đặt 2 tạo ra`0 2 3`. Cả hai đều tiêu thụ chính xác cùng một phổ và việc sắp xếp các câu trả lời cuối cùng sẽ đặt`0 1 3`trước`0 2 3`. 

Bất biến trung tâm đằng sau tất cả các trường hợp này là bảng tần số còn lại. Một nhánh không bao giờ được chấp nhận chỉ vì tọa độ của nó có vẻ hợp lý. Mỗi cặp mới được tạo phải sử dụng một bản sao phù hợp từ nhiều tập hợp đầu vào và mọi bản sao đã sử dụng sẽ được khôi phục khi nhánh bị hủy. Đây là điều ngăn cản việc xử lý không chính xác các khoảng cách lặp lại, khoảng cách điểm giữa và các giải pháp đối xứng.
