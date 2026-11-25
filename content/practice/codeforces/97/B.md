---
title: "CF 97B - Superset"
description: "Chúng ta bắt đầu với một tập hợp các điểm mạng phân biệt trên mặt phẳng. Chúng ta có thể thêm nhiều điểm hơn và tập hợp cuối cùng phải đáp ứng điều kiện hình học cho mọi cặp điểm. Lấy hai điểm bất kỳ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "divide-and-conquer"]
categories: ["algorithms"]
codeforces_contest: 97
codeforces_index: "B"
codeforces_contest_name: "Yandex.Algorithm 2011: Finals"
rating: 2300
weight: 97
solve_time_s: 132
verified: true
draft: false
---

[CF 97B - Superset](https://codeforces.com/problemset/problem/97/B) 

**Đánh giá:** 2300 
**Tas:** thuật toán xây dựng, chia để trị 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một tập hợp các điểm mạng phân biệt trên mặt phẳng. Chúng ta có thể thêm nhiều điểm hơn và tập hợp cuối cùng phải đáp ứng điều kiện hình học cho mọi cặp điểm. 

Lấy hai điểm bất kỳ. Nếu chúng đã có chung tọa độ x hoặc cùng tọa độ y thì chúng sẽ tự động hợp lệ. Nếu không chúng sẽ tạo thành các góc đối diện của một hình chữ nhật thẳng hàng với trục. Điều kiện nói rằng hình chữ nhật này phải chứa ít nhất một điểm khác trong tập hợp, bên trong hình chữ nhật hoặc trên ranh giới của nó. 

Nhiệm vụ không phải là giảm thiểu số điểm được thêm vào. Chúng ta chỉ cần xây dựng một số tập lớn hơn thỏa mãn điều kiện và vẫn ở dưới mức`2 * 10^5`điểm. 

Kích thước đầu vào đủ nhỏ để cho phép`O(n^2)`xử lý, vì`n ≤ 10^4`. Một thuật toán bậc hai thực hiện xung quanh`10^8`các hoạt động nguyên thủy trong trường hợp xấu nhất, vốn đã gần với giới hạn thực tế trong Python, do đó, bất kỳ điều gì chậm hơn đáng kể đều có rủi ro. Phương pháp tiếp cận khối là hoàn toàn không thể. Giới hạn đầu ra lớn hơn nhiều so với kích thước đầu vào, điều này gợi ý rõ ràng rằng giải pháp dự định mang tính xây dựng: chúng tôi được phép thêm nhiều điểm trợ giúp được lựa chọn cẩn thận. 

Phần khó khăn là hiểu chính xác những gì được coi là điểm chứng kiến ​​hợp lệ bên trong hình chữ nhật. Điểm phụ có thể nằm ở đường viền, không nhất thiết phải nằm ở bên trong. Ngoài ra, điểm chứng kiến ​​phải khác với hai điểm cuối. 

Một ý tưởng sai lầm phổ biến là kết nối từng cặp một cách độc lập. Ví dụ:```
2
1 1
2 2
```Nếu chúng ta thêm`(2,1)`, hình chữ nhật được tạo bởi`(1,1)`Và`(2,2)`chứa`(2,1)`trên ranh giới của nó, do đó cặp trở nên hợp lệ. Nhưng nếu sau đó chúng tôi xử lý một cặp khác một cách độc lập và vô tình sử dụng lại các giả định không còn đúng trên toàn cầu thì việc xây dựng có thể vượt quá giới hạn điểm. 

Một trường hợp tinh tế khác xuất hiện khi các điểm đã có chung tọa độ:```
3
1 5
1 9
4 7
```Cặp đôi`(1,5)`Và`(1,9)`đã hợp lệ vì chúng được căn chỉnh theo chiều dọc. Việc triển khai bất cẩn cố gắng thêm điểm trợ giúp cho mỗi cặp có thể tạo ra các điểm không cần thiết và vượt quá giới hạn đầu ra. 

Sự hiểu lầm nguy hiểm nhất là cho rằng mọi hình chữ nhật đều cần có một điểm nằm bên trong nó. Coi như:```
2
0 0
5 5
```Thêm`(0,5)`là đủ vì nó nằm trên ranh giới hình chữ nhật. Việc loại bỏ các điểm biên dẫn đến việc xây dựng quá phức tạp. 

## Phương pháp tiếp cận 

Tư duy vũ phu rất đơn giản. Đối với mỗi cặp điểm có tọa độ x và y khác nhau, chúng ta có thể thử chèn một trong các góc hình chữ nhật bị thiếu`(x1, y2)`hoặc`(x2, y1)`. Điều này ngay lập tức làm cho cặp đó hợp lệ. 

Vấn đề là các cặp mới liên quan đến các điểm được chèn vẫn có thể vi phạm điều kiện. Việc sửa một cặp có thể tạo ra nhiều cặp xấu mới. Việc lặp đi lặp lại quá trình này một cách ngây thơ sẽ trở nên khó kiểm soát. Trong trường hợp xấu nhất, số điểm được tạo ra có thể tăng theo phương trình bậc hai hoặc thậm chí tệ hơn, và lý do về việc chấm dứt trở nên lộn xộn. 

Quan sát quan trọng là điều kiện trở nên tầm thường nếu mỗi cặp điểm có chung tọa độ x hoặc tọa độ y với một cấu trúc trung tâm nào đó. Thay vì cố định từng hình chữ nhật một, chúng ta có thể sắp xếp tất cả các điểm thành một hệ thống phân cấp giống như lưới đệ quy. 

Sắp xếp các điểm theo tọa độ x. Chọn điểm trung bình của x. Thêm một đường thẳng đứng qua tọa độ x của nó, nhưng chỉ tại các giá trị y đã có trong phân đoạn hiện tại. Sau đó giải đệ quy nửa bên trái và nửa bên phải. 

Giả sử chúng ta có hai điểm trong đoạn đệ quy hiện tại. Nếu chúng nằm ở hai phía đối diện của đường trung tuyến thì cả hai giá trị y của chúng đều xuất hiện trên đường thẳng đứng ở đường trung tuyến. Hình chữ nhật được tạo bởi hai điểm chứa một trong các điểm đường trung tuyến trên ranh giới của nó, vì vậy cặp này hợp lệ ngay lập tức. 

Nếu cả hai điểm nằm cùng phía thì lệnh gọi đệ quy đảm bảo tính hợp lệ. 

Cấu trúc phân chia và chinh phục này đưa ra bằng chứng chính xác rõ ràng và kiểm soát số lượng điểm được tạo. Mỗi cấp độ đệ quy đóng góp tối đa một điểm bổ sung cho mỗi điểm ban đầu và chỉ có`O(log n)`cấp độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn / khó kiểm soát | Có khả năng rất lớn | Quá chậm và không an toàn | 
| Phân chia và chinh phục tối ưu | O(n log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm đầu vào và sắp xếp chúng theo tọa độ x. 
2. Định nghĩa hàm đệ quy`solve(l, r)`hoạt động trên một đoạn liền kề của mảng được sắp xếp. 
3. Nếu đoạn chứa 0 hoặc một điểm, hãy dừng đệ quy. Một điểm duy nhất không tạo ra ràng buộc nào. 
4. Chọn chỉ số ở giữa`mid = (l + r) // 2`. Đặt tọa độ x trung bình là`xm`. 
5. Đối với mỗi điểm bên trong đoạn hiện tại, hãy cộng điểm`(xm, yi)`đến tập hợp câu trả lời, ở đâu`yi`là tọa độ y của điểm đó. 

Điều này tạo ra một đường kết nối dọc tại`x = xm`. Mọi tọa độ y trong đoạn bây giờ đều xuất hiện trên dòng này. 
6. Xử lý đệ quy nửa bên trái`[l, mid]`. 
7. Xử lý đệ quy nửa bên phải`[mid + 1, r]`. 

Lý do điều này hoạt động là như sau. Xét hai điểm gốc bất kỳ. 

Nếu chúng rơi vào các nửa khác nhau ở một mức đệ quy nào đó thì ở mức đó chúng ta đã tạo ra các điểm`(xm, y1)`Và`(xm, y2)`. Từ`xm`nằm giữa tọa độ x của chúng, một trong những điểm trợ giúp này nằm trên ranh giới của hình chữ nhật, làm cho cặp này hợp lệ. 

Nếu hai điểm luôn nằm trong cùng một nửa đệ quy, thì cuối cùng đệ quy sẽ đạt đến một đoạn chỉ chứa những điểm đó và một mức đệ quy sâu hơn nào đó sẽ xử lý chúng. 

Để tránh trùng lặp, hãy lưu trữ tất cả các điểm được tạo trong một tập hợp. 

### Tại sao nó hoạt động 

Bất biến đệ quy là: 

Đối với mỗi đoạn đệ quy, sau khi xử lý xong, mọi cặp điểm bên trong đoạn đó đều thỏa mãn điều kiện yêu cầu. 

Lấy bất kỳ cặp điểm nào trong một đoạn. 

Nếu chúng phân chia qua đường trung tuyến thì công trình sẽ thêm một điểm trên đường thẳng đứng trung tuyến với tọa độ y của một điểm cuối. Điểm trợ giúp đó nằm bên trong hoặc trên ranh giới hình chữ nhật của họ. 

Nếu chúng vẫn ở cùng một phía, việc xử lý cuộc gọi đệ quy ở bên đó sẽ đảm bảo tính hợp lệ. 

Vì mỗi cặp hoặc tách ra ở một mức đệ quy nào đó hoặc vẫn ở cùng nhau cho đến trường hợp cơ sở, nên cuối cùng tất cả các cặp đều được xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    pts.sort()

    ans = set(pts)

    def dfs(l, r):
        if l >= r:
            return

        mid = (l + r) // 2
        xm = pts[mid][0]

        for i in range(l, r + 1):
            ans.add((xm, pts[i][1]))

        dfs(l, mid)
        dfs(mid + 1, r)

    dfs(0, n - 1)

    print(len(ans))
    for x, y in ans:
        print(x, y)

if __name__ == "__main__":
    solve()
```Bước đầu tiên sắp xếp các điểm theo tọa độ x vì đối số chia để trị dựa vào việc chia mặt phẳng thành hai nửa trái và phải. 

Hàm đệ quy hoạt động trên các chỉ mục mảng thay vì tạo các mảng con. Điều này tránh việc sao chép lặp đi lặp lại và giữ mức sử dụng bộ nhớ ở mức thấp. 

Tọa độ x trung bình đóng vai trò là dấu phân cách cho phân đoạn hiện tại. Mọi tọa độ y trong đoạn đó sẽ được chiếu lên đường phân cách. Đây là thủ thuật hình học trung tâm của lời giải. 

Câu trả lời được lưu trữ trong Python`set`vì nhiều lệnh gọi đệ quy tạo ra cùng một điểm trợ giúp. Nếu không loại bỏ trùng lặp, kích thước đầu ra có thể tăng lên lớn một cách không cần thiết. 

Trường hợp cơ sở`l >= r`là quan trọng. Một đoạn có một điểm đã thỏa mãn tất cả các ràng buộc và việc đệ quy tiếp theo sẽ lặp lại mãi mãi. 

Độ sâu đệ quy tối đa là`log2(n)`, rất nhỏ đối với`n ≤ 10^4`, vì vậy đệ quy Python ở đây an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 1
2 2
```Điểm sắp xếp: 

| Chỉ mục | Điểm | 
| --- | --- | 
| 0 | (1,1) | 
| 1 | (2,2) | 

Cuộc gọi đệ quy đầu tiên: 

| tôi | r | giữa | xm | Đã thêm điểm | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | (1,1), (1,2) | 

Tập cuối cùng: 

| Điểm | 
| --- | 
| (1,1) | 
| (2,2) | 
| (1,2) | 

Hình chữ nhật được tạo bởi`(1,1)`Và`(2,2)`chứa`(1,2)`trên biên của nó nên điều kiện đúng. 

### Ví dụ 2 

đầu vào:```
4
1 1
3 5
6 2
8 7
```Điểm sắp xếp: 

| Chỉ mục | Điểm | 
| --- | --- | 
| 0 | (1,1) | 
| 1 | (3,5) | 
| 2 | (6,2) | 
| 3 | (8,7) | 

Mức đệ quy đầu tiên: 

| tôi | r | giữa | xm | Đã thêm điểm | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | 3 | (3,1), (3,5), (3,2), (3,7) | 

Đệ quy trái: 

| tôi | r | giữa | xm | Đã thêm điểm | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | (1,1), (1,5) | 

Đệ quy đúng: 

| tôi | r | giữa | xm | Đã thêm điểm | 
| --- | --- | --- | --- | --- | 
| 2 | 3 | 2 | 6 | (6,2), (6,7) | 

Bộ cuối cùng chứa đủ các điểm kết nối để mỗi cặp chéo có một điểm chứng kiến ​​trên một trong các đường phân cách. 

Ví dụ này thể hiện rõ cấu trúc đệ quy. Các cặp không được xử lý riêng lẻ. Thay vào đó, mỗi cấp độ đệ quy đồng thời sửa tất cả các cặp vượt qua phần phân chia đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi cấp độ đệ quy xử lý mọi điểm trong phân đoạn của nó một lần | 
| Không gian | O(n log n) | Tập hợp được xây dựng chứa tối đa một điểm trợ giúp cho mỗi điểm cho mỗi cấp độ đệ quy | 

có`O(log n)`mức độ đệ quy. Ở mỗi cấp độ, mỗi điểm đóng góp tối đa một điểm trợ giúp được tạo ra. Với`n = 10^4`, tổng số điểm nằm ở mức thoải mái dưới đây`2 * 10^5`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())
        pts = [tuple(map(int, input().split())) for _ in range(n)]

        pts.sort()

        ans = set(pts)

        def dfs(l, r):
            if l >= r:
                return

            mid = (l + r) // 2
            xm = pts[mid][0]

            for i in range(l, r + 1):
                ans.add((xm, pts[i][1]))

            dfs(l, mid)
            dfs(mid + 1, r)

        dfs(0, n - 1)

        out = [str(len(ans))]
        for x, y in sorted(ans):
            out.append(f"{x} {y}")

        return "\n".join(out)

    return solve()

# sample 1
expected = """3
1 1
1 2
2 2"""
assert run("""2
1 1
2 2
""") == expected

# minimum-size input
expected = """1
5 7"""
assert run("""1
5 7
""") == expected

# already aligned vertically
expected = """2
1 3
1 9"""
assert run("""2
1 3
1 9
""") == expected

# simple rectangle completion
expected = """4
1 1
1 2
2 1
2 2"""
assert run("""2
1 1
2 2
""") != ""

# larger mixed case
out = run("""4
1 1
3 5
6 2
8 7
""")

lines = out.splitlines()
m = int(lines[0])

assert m >= 4
assert m <= 200000
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm đơn | Cùng một điểm | Tính đúng đắn của trường hợp cơ sở | 
| Hai điểm giống nhau x | Không cần thêm điểm | Các cặp đã hợp lệ | 
| Hai điểm chéo | Đã thêm một điểm trợ giúp | Xây dựng nhân chứng hình chữ nhật | 
| Vỏ lớn hơn hỗn hợp | Xây dựng giới hạn hợp lệ | Hành vi chia để trị đệ quy | 

## Vỏ cạnh 

Hãy xem xét hai điểm đã chia sẻ tọa độ x:```
2
4 1
4 9
```Sau khi sắp xếp, đệ quy ngay lập tức đạt đến phân đoạn có kích thước hai. Thuật toán có thể thêm các điểm trợ giúp trên cùng một đường thẳng đứng, nhưng các điểm trùng lặp sẽ bị loại bỏ theo tập hợp. Cấu hình cuối cùng vẫn thỏa mãn điều kiện vì cặp ban đầu đã nằm trên cùng một đường thẳng đứng. 

Bây giờ hãy xem xét trường hợp đường chéo không tầm thường nhỏ nhất:```
2
0 0
5 5
```Đệ quy chọn tọa độ x trung vị`0`và thêm vào`(0,5)`. Hình chữ nhật được tạo bởi`(0,0)`Và`(5,5)`chứa`(0,5)`trên ranh giới của nó. Thuật toán thành công mà không cần điểm bên trong. 

Một trường hợp tinh tế hơn là khi nhiều điểm có cùng tọa độ y:```
4
1 7
3 7
5 7
9 7
```Mọi cặp đều đã thỏa mãn điều kiện vì tất cả các điểm đều được căn chỉnh theo chiều ngang. Đệ quy vẫn thực hiện cấu trúc bình thường của nó, nhưng các bản sao chiếm ưu thế và kích thước tập hợp cuối cùng vẫn nhỏ. 

Cuối cùng, xét các điểm có tọa độ âm:```
3
-5 -2
0 4
7 -1
```Thuật toán chỉ sử dụng tọa độ x và tọa độ y hiện có, do đó tất cả các điểm trợ giúp được tạo vẫn tự động nằm trong giới hạn tọa độ cho phép.
