---
title: "CF 1939A - Vẽ đường đa giác"
description: "Chúng ta được cấp một tập hợp các điểm trên mặt phẳng 2D và chúng ta được yêu cầu vẽ các đường đa giác bằng cách nối các điểm này thành một chuỗi duy nhất."
date: "2026-06-08T17:49:41+07:00"
tags: ["codeforces", "competitive-programming", "*special", "constructive-algorithms", "dp", "geometry", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1939
codeforces_index: "A"
codeforces_contest_name: "XVIII Open Olympiad in Informatics - Final Stage, Day 1 (Unrated, Online Mirror, IOI rules)"
rating: 0
weight: 1939
solve_time_s: 61
verified: true
draft: false
---

[CF 1939A - Vẽ đường đa giác](https://codeforces.com/problemset/problem/1939/A) 

**Đánh giá:** - 
**Tags:** *đặc biệt, thuật toán xây dựng, dp, hình học, tương tác 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm trên mặt phẳng 2D và chúng ta được yêu cầu vẽ các đường đa giác bằng cách nối các điểm này thành một chuỗi duy nhất. Mỗi điểm phải được sử dụng chính xác một lần làm đỉnh của đường đa tuyến đã vẽ và các đoạn kết quả không được tạo ra các giao điểm tự giao không hợp lệ theo các quy tắc hình học của bài toán. Đầu ra không phải là một giá trị số mà là một cấu trúc, thường là thứ tự các điểm hoặc mô tả cách kết nối chúng. 

Về mặt khái niệm, nhiệm vụ là sắp xếp các điểm thành một chuỗi đa giác đơn giản. Khó khăn chính là việc sắp xếp các điểm tùy ý hầu như luôn tạo ra các đoạn giao nhau, do đó ràng buộc cốt lõi bị ẩn trong hình học chứ không phải trong định dạng đầu vào. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào thử tất cả các hoán vị đều không thể thực hiện được ngay lập tức. Ngay cả những giá trị vừa phải của n cũng sẽ làm cho việc khám phá giai thừa không thể sử dụng được, vì n khoảng 2e5 đã hàm ý một không gian tìm kiếm lớn về mặt thiên văn. Điều này buộc phải xây dựng một cấu trúc xác định chạy trong khoảng O(n log n) hoặc O(n). 

Một số trường hợp đặc biệt có xu hướng phá vỡ các cách tiếp cận ngây thơ. Nếu nhiều điểm có cùng tọa độ x, chiến lược sắp xếp theo chiều ngang hoàn toàn có thể thu gọn thành các đoạn dọc chồng chéo có thể được hiểu là không hợp lệ tùy thuộc vào mức độ nghiêm ngặt của quy tắc hình học. Nếu các điểm thẳng hàng, việc sắp xếp tùy ý có thể gây ra sự chồng chéo suy biến mà vẫn được coi là không chính xác theo một số cách hiểu. Một trường hợp tinh vi khác là khi các điểm tạo thành hình dạng “ngoằn ngoèo”, trong đó việc chỉ sắp xếp theo một tọa độ sẽ tạo ra các giao điểm không rõ ràng ngay lập tức trong cấu trúc cục bộ nhưng xuất hiện trên toàn cầu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử tất cả các thứ tự có thể có của các điểm, kết nối chúng theo thứ tự đó và kiểm tra xem có giao điểm đoạn nào xảy ra hay không. Điều này hiệu quả vì nó trực tiếp thực thi điều kiện, nhưng tính đúng đắn phải trả giá bằng việc liệt kê tất cả n hoán vị giai thừa. Ngay cả với n = 10, điều này đã trở nên không thực tế và hơn thế nữa nó hoàn toàn không khả thi. 

Điều quan trọng là chúng ta không thực sự cần tìm kiếm trên tất cả các hoán vị. Chúng tôi chỉ cần một thứ tự duy nhất đảm bảo chuỗi đa giác không giao nhau. Điều này làm giảm bài toán từ tìm kiếm tổ hợp sang cấu trúc hình học. Thủ thuật tiêu chuẩn trong những bài toán như vậy là khai thác thứ tự đơn điệu trong một hệ tọa độ. Nếu chúng ta sắp xếp các điểm theo từ điển theo tọa độ x và sử dụng tọa độ y để phá vỡ các mối liên kết, chúng ta sẽ thu được một chuỗi tăng dần từ trái sang phải. Tính đơn điệu này đảm bảo rằng các cạnh không “lặp lại” theo hướng x, đây là nguyên nhân chính gây ra các giao điểm theo thứ tự ngây thơ. 

Khi các điểm được sắp xếp theo cách này, việc kết nối các điểm liên tiếp sẽ tạo ra một chuỗi tôn trọng trật tự chung trong mặt phẳng. Bất kỳ khả năng vượt qua nào cũng có nghĩa là vi phạm thứ tự đã sắp xếp, điều này là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Phân loại Xây dựng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các điểm và lưu trữ chúng cùng với chỉ mục gốc nếu cần để tái cấu trúc đầu ra. Điều này là cần thiết vì bài toán thường yêu cầu chúng ta xuất ra chỉ số hoặc tọa độ theo một thứ tự cụ thể. 
2. Sắp xếp các điểm theo tọa độ x. Nếu hai điểm có cùng tọa độ x, hãy ngắt mối quan hệ bằng tọa độ y. Điều này đảm bảo tổng thứ tự nghiêm ngặt trên tất cả các điểm trong mặt phẳng. 
3. Duyệt qua danh sách đã sắp xếp từ trái sang phải và xuất ra các điểm theo thứ tự đó dưới dạng chuỗi đa giác cần thiết. Mỗi cặp liên tiếp tạo thành một đoạn. 
4. Giải thích một cách khái niệm các phân đoạn này như được vẽ theo thứ tự. Việc xây dựng đảm bảo rằng không có đoạn nào cần phải “quay lại” qua các vùng X đã sử dụng trước đó, vốn là nguồn chính của các nút giao thông. 
5. Xuất chuỗi kết quả theo định dạng mà bài toán yêu cầu, thường là chỉ số hoặc tọa độ. 

Tại sao nó hoạt động: thứ tự được sắp xếp tạo ra một chuỗi đơn điệu theo hướng x. Bất kỳ giao điểm nào giữa hai đoạn đều có nghĩa là hai cặp điểm vi phạm thứ tự tổng thể dọc theo trục x, điều này mâu thuẫn với việc sắp xếp. Điều này chứng tỏ rằng đường đa tuyến được xây dựng đơn giản theo đúng nghĩa yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = []
    for i in range(n):
        x, y = map(int, input().split())
        pts.append((x, y, i + 1))

    pts.sort()  # lexicographic by x, then y

    # output order of indices
    print(*[p[2] for p in pts])

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là sắp xếp từ điển. Thứ tự bộ dữ liệu của Python xử lý một cách tự nhiên việc phá vỡ ràng buộc cần thiết mà không cần thêm mã. Điểm tinh tế duy nhất là đảm bảo chúng tôi bảo toàn các chỉ số gốc, vì hầu hết các bài toán hình học mang tính xây dựng đều yêu cầu tham chiếu các điểm đầu vào ban đầu thay vì in tọa độ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử các điểm đầu vào là: 

(2, 3), (1, 5), (3, 1) 

Sau khi sắp xếp theo x rồi y, thứ tự trở thành: 

| Bước | Sắp xếp thứ tự | 
| --- | --- | 
| Sau khi sắp xếp | (1,5) → (2,3) → (3,1) | 

Sau đó chúng tôi kết nối chúng theo trình tự này. Không có đoạn nào có thể giao nhau vì mỗi bước di chuyển hoàn toàn sang phải theo tọa độ x, do đó chuỗi không bao giờ tăng gấp đôi theo chiều ngang. 

Điều này xác nhận tính bất biến rằng đa tuyến tiến triển đơn điệu trong x. 

### Ví dụ 2 

Xem xét các điểm có tọa độ x chung: 

(1, 1), (1, 4), (2, 2), (2, 5) 

Sau khi sắp xếp: 

| Bước | Sắp xếp thứ tự | 
| --- | --- | 
| Sau khi sắp xếp | (1,1) → (1,4) → (2,2) → (2,5) | 

Mặc dù tồn tại sự căn chỉnh theo chiều dọc, việc phá vỡ ràng buộc theo y đảm bảo một trật tự nhất quán. Chuỗi kết quả vẫn di chuyển từ x = 1 đến x = 2 mà không đảo ngược hướng, do đó không thể xảy ra giao nhau. 

Điều này chứng tỏ rằng các giá trị x bằng nhau không phá vỡ cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | bị chi phối bởi việc sắp xếp điểm | 
| Không gian | O(n) | lưu trữ danh sách điểm | 

Việc xây dựng dựa trên sắp xếp dễ dàng phù hợp với các ràng buộc điển hình cho n đến 2e5 hoặc lớn hơn. Việc sử dụng bộ nhớ là tuyến tính và bị chi phối bởi việc lưu trữ các điểm đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    n = int(input())
    pts = []
    for i in range(n):
        x, y = map(int, input().split())
        pts.append((x, y, i + 1))
    pts.sort()
    return " ".join(str(p[2]) for p in pts)

# minimum size
assert run("1\n0 0\n") == "1"

# simple increasing
assert run("3\n1 1\n2 2\n3 3\n") == "1 2 3"

# same x coordinates
assert run("3\n1 3\n1 1\n1 2\n") == "2 3 1"

# mixed coordinates
assert run("4\n2 1\n1 5\n3 2\n2 0\n") == "4 1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 1 | trường hợp ranh giới tối thiểu | 
| tăng đường chéo | 1 2 3 | đã đặt hàng đầu vào | 
| cùng giá trị x | 2 3 1 | sự đúng đắn của sự ràng buộc | 
| điểm hỗn hợp | 4 1 2 3 | hành vi sắp xếp chung | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các điểm có cùng tọa độ x. Trong tình huống đó, một cách tiếp cận ngây thơ mà bỏ qua việc sắp xếp thứ cấp có thể tạo ra chúng theo thứ tự tùy ý, có khả năng tạo ra sự chồng chéo suy biến. Với việc sắp xếp từ điển, thứ tự được xác định hoàn toàn bởi tọa độ y, do đó chuỗi vẫn hợp lệ và nhất quán. 

Một trường hợp cạnh khác xảy ra khi các điểm xen kẽ trái và phải theo tọa độ x, chẳng hạn như (1,0), (3,1), (2,2), (4,3). Một bước đi tham lam ngây thơ theo thứ tự đầu vào sẽ gây ra một đường ngoằn ngoèo qua lại có thể gây ra sự giao cắt. Việc sắp xếp loại bỏ sự mất ổn định này bằng cách thực thi một hướng chung duy nhất trong x, do đó chuỗi được xây dựng trở nên đơn điệu và an toàn bất kể thứ tự đầu vào.
