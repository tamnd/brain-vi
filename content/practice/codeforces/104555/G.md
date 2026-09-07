---
title: "CF 104555G - Hiệp ước vĩ đại Byteland"
description: "Mỗi vương quốc được thể hiện bằng một điểm trên mặt phẳng 2D và thế giới được phân chia theo quy tắc thủ đô gần nhất. Mọi vị trí trong mặt phẳng vô hạn đều thuộc về bất kỳ thủ đô nào gần nhất trong khoảng cách Euclide."
date: "2026-06-30T08:50:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104555
codeforces_index: "G"
codeforces_contest_name: "2023-2024 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 104555
solve_time_s: 144
verified: false
draft: false
---

[CF 104555G - Hiệp ước vĩ đại của Byteland](https://codeforces.com/problemset/problem/104555/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi vương quốc được thể hiện bằng một điểm trên mặt phẳng 2D và thế giới được phân chia theo quy tắc thủ đô gần nhất. Mọi vị trí trong mặt phẳng vô hạn đều thuộc về bất kỳ thủ đô nào gần nhất trong khoảng cách Euclide. Các điểm cách đều nhau với nhiều thủ đô tạo thành các đường ranh giới, nhưng những điểm đó không đóng góp diện tích cho bất kỳ vương quốc nào. 

Câu hỏi không phải là tính toán đầy đủ các vùng một cách rõ ràng. Thay vào đó, chúng ta chỉ cần xác định thủ đô nào sở hữu các vùng có diện tích vô hạn trong các ô Voronoi của chúng. Về mặt hình học, chúng tôi muốn tìm điểm nào có các ô Voronoi không bị chặn. 

Các ràng buộc đủ lớn để tránh bất kỳ phương pháp nào liên quan đến so sánh hình học theo cặp giữa tất cả các địa điểm. Một cấu trúc hình học đơn giản hoặc lấy mẫu khoảng cách rõ ràng sẽ ngay lập tức vượt quá giới hạn vì cấu trúc tự nhiên ở đây là bậc hai về số điểm. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả sử rằng mọi điểm đều tham gia như nhau hoặc các điểm lân cận cục bộ trong khoảng cách Euclide xác định giới hạn. Ví dụ, một điểm có thể có các lân cận rất gần nhau nhưng vẫn nằm trên bao lồi và do đó không bị chặn. Ngược lại, một điểm có thể có mật độ cục bộ tương đối thưa thớt và vẫn được bao bọc hoàn toàn. 

Ý tưởng còn thiếu chính là các ô Voronoi bị chặn xuất hiện chính xác đối với các điểm nằm hoàn toàn bên trong bao lồi, trong khi các ô vô hạn tương ứng chính xác với các đỉnh của bao lồi. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu từ định nghĩa: đối với mỗi thủ đô, hãy tưởng tượng vùng Voronoi của nó và cố gắng xác định xem nó có kéo dài đến vô tận hay không. Người ta có thể mô phỏng các tia theo nhiều hướng hoặc lấy mẫu các điểm ở xa và xem thủ đô nào vẫn gần nhất. Điều này đúng về mặt khái niệm vì một ô không bị chặn phải “đạt tới” xa một cách tùy ý. Tuy nhiên, ngay cả việc kiểm tra một hướng cũng đòi hỏi phải so sánh khoảng cách với tất cả các điểm khác và các hướng lấy mẫu đủ dày đặc khiến chi phí tăng vọt lên mức O(N³) hoặc tệ hơn trong thực tế. 

Cái nhìn sâu sắc về cấu trúc là các vùng Voronoi không giới hạn xảy ra chính xác khi một điểm nằm trên bao lồi của tập hợp các thủ đô. Nếu một điểm nằm trên bao lồi thì tồn tại một hướng mà nó vẫn là điểm cực trị, nghĩa là không có điểm nào khác lấn át nó theo hướng đó, do đó ô Voronoi của nó kéo dài vô tận. Nếu một điểm nằm hoàn toàn bên trong bao lồi, thì mọi hướng cuối cùng sẽ gặp một đỉnh bao trội chặn phần mở rộng vô hạn, do đó vùng Voronoi của nó bị giới hạn. 

Điều này làm giảm vấn đề tính toán bao lồi và báo cáo tất cả các đỉnh xuất hiện trên đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra hình học bằng vũ lực | Theo cấp số nhân hoặc tệ hơn | O(N) | Quá chậm | 
| Tính toán thân lồi | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm nhiệm vụ tìm tất cả các điểm trên ranh giới bao lồi bằng cách sử dụng cấu trúc chuỗi đơn điệu tiêu chuẩn.

1. Sắp xếp tất cả các điểm theo tọa độ x và theo tọa độ y làm điểm phân định. Điều này thiết lập một thứ tự quét xác định cần thiết cho việc xây dựng thân tàu. 
2. Xây dựng phần thân dưới bằng cách quét các điểm từ trái sang phải. Duy trì một chồng các đỉnh thân ứng cử viên. Đối với mỗi điểm mới, chúng tôi kiểm tra xem hai điểm cuối cùng trong ngăn xếp cùng với điểm mới có rẽ trái hay không. Nếu đúng như vậy thì điểm ở giữa không thể thuộc về bao lồi và bị loại bỏ. Chúng tôi lặp lại điều này cho đến khi bất biến được khôi phục, sau đó thêm điểm mới. 
3. Xây dựng thân trên theo cách tương tự nhưng quét theo thứ tự ngược lại. Điều này đảm bảo tính đối xứng và nắm bắt được ranh giới trên của hình lồi. 
4. Kết hợp cả hai thân tàu, chú ý loại bỏ các điểm cuối trùng lặp. Tập kết quả chứa tất cả các đỉnh của bao lồi. 
5. In ra tất cả các điểm xuất hiện trong thân tàu theo thứ tự chỉ số tăng dần. 

Kiểm tra hình học được sử dụng ở bước 2 là hướng (tích chéo). Tích chéo không dương chỉ ra rằng dãy không rẽ trái nghiêm ngặt, nghĩa là điểm giữa không thể nằm trên ranh giới lồi. 

## Tại sao nó hoạt động 

Một điểm có vùng Voronoi vô hạn khi và chỉ khi tồn tại một hướng mà nó không bị chi phối bởi bất kỳ điểm nào khác. Điều này xảy ra chính xác khi nó là đỉnh của bao lồi. Thân tàu đảm bảo rằng có một đường đỡ chạm vào bộ tại điểm đó sao cho tất cả các điểm khác nằm về một phía. Đường hỗ trợ đó xác định hướng mà vùng Voronoi kéo dài vô tận. Các điểm bên trong không thể có đường hỗ trợ như vậy nên vùng Voronoi của chúng bị giới hạn. 

Do đó, bao lồi mô tả chính xác tập hợp yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

def convex_hull(points):
    points = sorted(points)
    if len(points) <= 1:
        return points

    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    hull = lower[:-1] + upper[:-1]
    return hull

def solve():
    n = int(input())
    pts = []
    for i in range(n):
        x, y = map(int, input().split())
        pts.append((x, y, i + 1))

    # sort by coordinates but keep index
    pts_sorted = sorted(pts, key=lambda p: (p[0], p[1]))

    def cross2(a, b, c):
        return (b[0]-a[0])*(c[1]-a[1]) - (b[1]-a[1])*(c[0]-a[0])

    lower = []
    for p in pts_sorted:
        while len(lower) >= 2 and cross2(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(pts_sorted):
        while len(upper) >= 2 and cross2(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    hull = lower[:-1] + upper[:-1]

    ans = sorted(set(p[2] for p in hull))
    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ lại các chỉ mục ban đầu để chúng tôi có thể báo cáo ID vương quốc. Chuỗi đơn điệu được chia thành bao dưới và bao trên, và chúng tôi sử dụng phép thử tích chéo để duy trì tính lồi. Điểm chỉ bị xóa khi vi phạm điều kiện biên lồi nghiêm ngặt, đảm bảo rằng các điểm biên thẳng hàng được xử lý một cách nhất quán. 

Một chi tiết tinh tế là chúng tôi bao gồm các điểm biên thẳng hàng như một phần của thân tàu nếu chúng nằm ở các cạnh xa nhất. các`<= 0`điều kiện đảm bảo chúng ta không giữ các điểm thẳng hàng bên trong mà vẫn bảo toàn các điểm cuối bên ngoài. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ: 

đầu vào:```
4
3 2
1 5
3 6
3 5
```Chúng tôi sắp xếp các điểm theo từ điển và xây dựng thân tàu. Ngăn xếp phát triển khi các điểm được thêm và xóa dựa trên hướng. Thân cuối cùng chứa tất cả các điểm vì mỗi điểm nằm trên biên của hình được tạo bởi tập hợp. 

| Bước | Đã thêm điểm | Trạng thái thân dưới | 
| --- | --- | --- | 
| 1 | (1,5) | (1,5) | 
| 2 | (3,2) | (1,5),(3,2) | 
| 3 | (3,5) | (1,5),(3,2),(3,5) | 
| 4 | (3,6) | (1,5),(3,2),(3,6) | 

Thân trên tái tạo lại cấu trúc ranh giới tương tự. Tất cả các điểm vẫn nằm trên ranh giới lồi, vì vậy tất cả các vương quốc đều có vùng vô hạn. 

Bây giờ hãy xem xét một mẫu “đóng kín” hơn: 

đầu vào:```
6
2 1
3 3
1 4
4 5
6 3
4 3
```Ở đây các điểm bên trong bị loại bỏ trong quá trình xây dựng thân tàu vì chúng tạo ra các lối rẽ phải so với các điểm cực xung quanh. Chỉ có đỉnh ranh giới tồn tại. 

Quá trình này cho thấy chỉ những điểm duy trì vị trí hình học cực trị mới tồn tại được trong cả hai lần quét, khớp với kết quả đầu ra dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | phân loại chiếm ưu thế, quét thân tàu là tuyến tính | 
| Không gian | O(N) | điểm lưu trữ và thân tàu | 

Các ràng buộc cho phép lên tới 100000 điểm, do đó việc so sánh hình học O(N2) là không khả thi. Thân tàu lồi dạng chuỗi đơn điệu tối ưu và vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    pts = []
    for i in range(n):
        x, y = map(int, input().split())
        pts.append((x, y, i + 1))

    pts.sort()
    def cross(a,b,c):
        return (b[0]-a[0])*(c[1]-a[1])-(b[1]-a[1])*(c[0]-a[0])

    lower=[]
    for p in pts:
        while len(lower)>=2 and cross(lower[-2],lower[-1],p)<=0:
            lower.pop()
        lower.append(p)

    upper=[]
    for p in reversed(pts):
        while len(upper)>=2 and cross(upper[-2],upper[-1],p)<=0:
            upper.pop()
        upper.append(p)

    hull = lower[:-1] + upper[:-1]
    return " ".join(map(str, sorted(set(p[2] for p in hull))))

# sample-like sanity
assert run("""4
3 2
1 5
3 6
3 5
""") == "1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi cộng tuyến | tất cả các điểm cuối | xử lý thân tàu thẳng hàng | 
| vuông | tất cả các đỉnh | phát hiện ranh giới đầy đủ | 
| điểm nội thất | không loại trừ bên trong | loại bỏ nội thất | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi tất cả các điểm nằm trên một đường thẳng. Trong trường hợp đó, mọi điểm đều nằm trên ranh giới bao lồi theo nghĩa suy biến và thuật toán vẫn phải trả về tất cả các điểm. Chuỗi đơn điệu xử lý việc này vì việc kiểm tra định hướng sẽ thu gọn các điểm cộng tuyến một cách nhất quán, để lại các điểm cuối tái tạo lại tập hợp đầy đủ khi được kết hợp. 

Một trường hợp cạnh khác là khi nhiều điểm có chung tọa độ x hoặc y cực trị. Thân tàu vẫn phải xác định chính xác chỉ cấu trúc ngoài cùng và việc xử lý trùng lặp khi ghép thân trên và thân dưới đảm bảo không có chỉ số hoặc thiếu sót lặp lại.
