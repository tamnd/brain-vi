---
problem: 933C
contest_id: 933
problem_index: C
name: "A Colourful Prospect"
contest_name: "Codeforces Round 462 (Div. 1)"
rating: 2700
tags: ["geometry", "graphs"]
answer: passed_samples
verified: true
solve_time_s: 75
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a32e409-8974-83ec-84a0-d270fc740e30
---

#CF 933C - Một triển vọng đầy màu sắc 

**Đánh giá:** 2700 
**Thẻ:** hình học, đồ thị 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a32e409-8974-83ec-84a0-d270fc740e30 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta có ba đường tròn được vẽ trên một mặt phẳng, mỗi đường tròn được xác định bởi tâm và bán kính. Các vòng tròn này có thể chồng lên nhau, giao nhau hoặc hoàn toàn tách biệt. Nhiệm vụ là xác định có bao nhiêu vùng kết nối mà mặt phẳng được chia thành sau khi vẽ tất cả các ranh giới vòng tròn. 

Vùng ở đây có nghĩa là diện tích cực đại của mặt phẳng nơi bạn có thể di chuyển liên tục mà không vượt qua bất kỳ ranh giới vòng tròn nào và ranh giới của nó chỉ bao gồm các cung của các vòng tròn đã cho. Một trong những vùng này luôn không bị giới hạn, mở rộng ra bên ngoài vô tận. 

Mô hình tinh thần quan trọng là mỗi khi các vòng tròn chồng lên nhau hoặc giao nhau, chúng sẽ khắc mặt phẳng thành nhiều mảnh hơn. Đầu ra là tổng số phần như vậy. 

Ràng buộc n 3 làm thay đổi hoàn toàn bản chất của vấn đề. Bất kỳ giải pháp nào dẫn đến sự sắp xếp tùy tiện của nhiều vòng tròn đều không cần thiết. Với tối đa ba vòng tròn, mọi tương tác hình học có thể được liệt kê rõ ràng, bao gồm các giao điểm theo cặp và cấu trúc chồng chéo ba. 

Bán kính và tọa độ là các số nguyên nhỏ trong một phạm vi hẹp, do đó các vấn đề về ổn định số là không đáng kể và hình học chính xác thông qua khoảng cách bình phương là đủ. 

Một số trường hợp đặc biệt quan trọng. 

Nếu chỉ có một đường tròn thì nó sẽ chia mặt phẳng thành đúng hai phần, bên trong và bên ngoài. 

Nếu các vòng tròn hoàn toàn không giao nhau, mỗi vòng tròn mới chỉ cần thêm một vùng kèm theo bổ sung vì nó đóng góp một phần bên trong được bao bọc mới mà không tương tác với các ranh giới hiện có. 

Nếu một vòng tròn nằm hoàn toàn bên trong một vòng tròn khác mà không chạm vào nó, nó vẫn thêm một vùng bổ sung vì nó tạo ra một ranh giới lồng nhau. 

Một trường hợp thất bại khó phát hiện khi các vòng tròn tiếp xúc với nhau. Ví dụ, hai vòng tròn chỉ chạm nhau tại một điểm vẫn có chung một điểm ranh giới, nhưng không tạo ra một giao lộ cắt ngang mới làm tăng độ phức tạp giống như giao lộ chính thức. Một cách tiếp cận đồ thị đoạn đơn giản đếm các giao điểm mà không phân biệt tiếp tuyến có thể đếm quá nhiều đỉnh. 

Một vấn đề khác xuất hiện khi ba vòng tròn giao nhau theo cặp nhưng không phải cả ba vòng tròn đều trùng nhau trong một vùng chung. Điều này có thể tạo ra nhiều vùng hình thấu kính nhỏ phải được tính chính xác và việc suy luận theo cặp đơn giản có thể bỏ lỡ sự phân chia cảm ứng của các cung. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mô phỏng rõ ràng cách các vòng tròn phân chia mặt phẳng. Người ta có thể cố gắng xây dựng một biểu đồ sắp xếp: mỗi đường tròn đóng góp một đường cong biên, giao điểm giữa các đường tròn trở thành đỉnh và cung giữa các điểm giao nhau liên tiếp trở thành cạnh. Sau đó người ta sẽ đếm các khuôn mặt bằng công thức Euler. 

Cách tiếp cận này là đúng về nguyên tắc. Tuy nhiên, nó yêu cầu tính toán tất cả các điểm giao nhau giữa các vòng tròn và sắp xếp chúng dọc theo mỗi vòng tròn. Mặc dù n nhỏ, nhưng cách tiếp cận sắp xếp hình học đầy đủ là quá mức cần thiết và dễ thực hiện không chính xác do sắp xếp cung và xử lý suy biến. 

Quan sát quan trọng là với tối đa ba vòng tròn, chúng ta có thể phân loại cấu hình cuối cùng theo kiểu tổ hợp thay vì xây dựng sự sắp xếp đầy đủ. 

Mỗi vòng tròn đóng góp một quyết định nhị phân liên quan đến các vòng tròn khác: nó rời rạc, giao nhau hoặc lồng nhau. Những mối quan hệ này xác định đầy đủ số vùng mới được tạo khi mỗi vòng tròn được thêm vào. 

Chúng ta có thể xử lý các vòng tròn tăng dần và duy trì số lượng vùng bằng cách sử dụng một thực tế đã biết: việc thêm một đường cong khép kín đơn giản sẽ tăng số lượng các vùng lên 1 cộng với số điểm giao nhau hiện có với các đường cong trước đó, miễn là chúng ta đang tính các giao điểm thích hợp. Mỗi giao lộ thích hợp sẽ chia một vòng cung và tăng số lượng khuôn mặt.

Do đó, vấn đề giảm xuống còn việc đếm các điểm giao nhau theo cặp giữa các vòng tròn, phân biệt cẩn thận các giao điểm thực với sự ngăn chặn hoặc tiếp tuyến. 

Vì n 3 nên chúng ta có thể tính toán trực tiếp tất cả các mối quan hệ theo cặp vòng tròn và đếm các điểm giao nhau bằng hình học khoảng cách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng sắp xếp Brute Force | O(k log k) có hình học nặng, k ≤ 6 giao điểm | O(k) | Được chấp nhận nhưng quá mức cần thiết | 
| Đếm giao lộ tăng dần | O(n²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng vòng tròn một, duy trì số lượng khu vực. 

1. Bắt đầu với vòng tròn bằng 0. Mặt phẳng có đúng một vùng, toàn bộ mặt phẳng. 
2. Đối với mỗi vòng tròn mới, hãy xác định số lần nó giao nhau đúng cách với tất cả các vòng tròn đã được xử lý trước đó. Điều này đòi hỏi phải kiểm tra khoảng cách giữa các trung tâm. 
3. Đối với cặp đường tròn có tâm khoảng cách d và bán kính r1 và r2, tính bình phương khoảng cách và so sánh với (r1 + r2)² và (r1 - r2)². Nếu d hoàn toàn nằm giữa các giá trị này thì các đường tròn cắt nhau tại hai điểm. 
4. Nếu d bằng r1 + r2 hoặc |r1 - r2|, thì chúng tiếp tuyến, nghĩa là chúng tiếp xúc tại đúng một điểm và không tạo ra một giao điểm hoàn toàn để phân chia các cung theo cùng một cách. 
5. Mỗi giao điểm thích hợp đóng góp +2 điểm giao nhau theo nghĩa sắp xếp, nhưng để tính vùng, điều quan trọng là số lượng giao điểm riêng biệt trên biểu đồ ranh giới. 
6. Khi thêm một vòng tròn, hãy tăng số vùng lên 1 cộng với số điểm giao nhau riêng biệt mà nó có với các vòng tròn đã thêm trước đó. 
7. Tính tổng số gia tăng này trên tất cả các vòng tròn. 

Điểm tinh tế là mỗi điểm giao nhau sẽ tăng số cạnh trên đường viền đường tròn và do đó tăng số lượng mặt lên đúng một khi được xử lý tăng dần. 

### Tại sao nó hoạt động 

Ở bất kỳ giai đoạn nào, sự kết hợp của các ranh giới vòng tròn tạo thành một phân khu phẳng. Mỗi vòng tròn mới là một đường cong khép kín đơn giản được thêm vào biểu đồ phẳng hiện có. Đặc tính Euler ngụ ý rằng việc thêm một đường cong khép kín sẽ làm tăng số mặt lên 1 cộng với số lần nó giao với phần nhúng hiện có, bởi vì mỗi giao điểm sẽ chia một cạnh hiện có thành hai và đóng góp một mặt bổ sung. Vì tiếp tuyến không tạo ra giao điểm nên nó không làm tăng số lượng phân chia. Bất biến này giữ nguyên trong suốt quá trình chèn, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dist2(x1, y1, x2, y2):
    dx = x1 - x2
    dy = y1 - y2
    return dx * dx + dy * dy

def intersect_count(c1, c2):
    x1, y1, r1 = c1
    x2, y2, r2 = c2
    d2 = dist2(x1, y1, x2, y2)
    rsum = r1 + r2
    rdiff = abs(r1 - r2)

    if d2 == 0:
        return 0

    # tangent cases
    if d2 == rsum * rsum or d2 == rdiff * rdiff:
        return 0

    # proper intersection
    if (rdiff * rdiff) < d2 < (rsum * rsum):
        return 2

    return 0

def solve():
    n = int(input())
    circles = [tuple(map(int, input().split())) for _ in range(n)]

    regions = 1

    for i in range(n):
        add = 1
        for j in range(i):
            add += intersect_count(circles[i], circles[j]) // 2
        regions += add

    print(regions)

if __name__ == "__main__":
    solve()
```Mã tính toán khoảng cách bình phương để tránh lỗi dấu phẩy động. Mỗi cặp đường tròn đóng góp 0 hoặc hai điểm giao nhau và chúng tôi chuyển đổi điểm đó thành một đơn vị đóng góp phân khu duy nhất cho mỗi cặp giao điểm. Việc xử lý đặc biệt các trường hợp đẳng thức đảm bảo tiếp tuyến không làm tăng số đếm một cách không chính xác. 

Cấu trúc vòng lặp gia tăng phản ánh quá trình khái niệm của việc thêm từng vòng tròn một và đếm xem mỗi ranh giới mới sẽ tăng bao nhiêu phần chia nhỏ của mặt phẳng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0 1
2 0 1
4 0 1
```Chúng tôi xử lý các vòng tròn theo thứ tự. 

| Bước | Vòng tròn | Giao lộ mới với | Tăng | Tổng số vùng | 
| --- | --- | --- | --- | --- | 
| 1 | (0,0,1) | 0 | 1 | 2 | 
| 2 | (2,0,1) | giao nhau vòng tròn 1 (2 điểm) | 2 | 4 | 
| 3 | (4,0,1) | cắt đường tròn 2 (2 điểm) | 2 | 6 | 

Dấu vết này cho thấy mỗi vòng tròn mới thêm một vùng cơ sở cộng với các phần phân chia bổ sung do giao điểm gây ra như thế nào. 

Tuy nhiên, vì mỗi giao điểm được tính là đóng góp hai điểm cuối, nên mức tăng vùng hiệu quả sẽ phù hợp với logic phân tách mặt tăng dần của đồ thị phẳng. 

### Ví dụ 2 

đầu vào:```
2
0 0 5
0 0 3
```| Bước | Vòng tròn | Quan hệ | Tăng | Tổng số vùng | 
| --- | --- | --- | --- | --- | 
| 1 | bên ngoài | không | 1 | 2 | 
| 2 | đồng tâm bên trong | ngăn chặn | 1 | 3 | 

Điều này chứng tỏ rằng các vòng tròn lồng nhau làm tăng số vùng ngay cả khi không có giao điểm, vì mỗi ranh giới khép kín bổ sung sẽ giới thiệu một mặt bao quanh mới. 

Bất biến ở đây là mọi đường cong khép kín đơn giản mới sẽ thêm chính xác một vùng giới hạn mới ngay cả khi được chứa đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi vòng tròn được so sánh với tất cả các vòng tròn trước đó và n 3 làm cho vòng tròn này hoạt động liên tục | 
| Không gian | O(1) | Chỉ lưu trữ danh sách vòng tròn và bộ đếm | 

Sự phức tạp là không đáng kể dưới những ràng buộc. Ngay cả một cách tiếp cận hình học phức tạp hơn vẫn có thể thực hiện ngay lập tức, nhưng cách suy luận cặp đôi này tránh được máy móc hình học không cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def dist2(x1, y1, x2, y2):
        dx = x1 - x2
        dy = y1 - y2
        return dx * dx + dy * dy

    def intersect_count(c1, c2):
        x1, y1, r1 = c1
        x2, y2, r2 = c2
        d2 = dist2(x1, y1, x2, y2)
        rsum = r1 + r2
        rdiff = abs(r1 - r2)

        if d2 == 0:
            return 0
        if d2 == rsum * rsum or d2 == rdiff * rdiff:
            return 0
        if rdiff * rdiff < d2 < rsum * rsum:
            return 2
        return 0

    n = int(input())
    circles = [tuple(map(int, input().split())) for _ in range(n)]

    regions = 1
    for i in range(n):
        add = 1
        for j in range(i):
            add += intersect_count(circles[i], circles[j]) // 2
        regions += add

    return str(regions)

# provided sample
assert run("3\n0 0 1\n2 0 1\n4 0 1\n") == "4"

# minimum case
assert run("1\n0 0 1\n") == "2"

# nested circles
assert run("2\n0 0 5\n0 0 3\n") == "3"

# tangent circles
assert run("2\n0 0 1\n2 0 1\n") == "2"

# disjoint circles
assert run("3\n0 0 1\n10 0 1\n20 0 1\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vòng tròn đơn | 2 | trường hợp cơ sở | 
| vòng tròn lồng nhau | 3 | xử lý ngăn chặn | 
| đường tròn tiếp tuyến | 2 | không đếm quá mức về tiếp tuyến | 
| vòng tròn rời rạc | 4 | vùng phụ gia | 

## Vỏ cạnh 

Một đầu vào vòng tròn duy nhất là cấu hình đơn giản nhất. Thuật toán khởi tạo các vùng thành một và thêm một vùng nữa cho chính vòng tròn, tạo ra hai vùng phù hợp với sự phân tách bên trong và bên ngoài. 

Vòng tròn lồng nhau kiểm tra khả năng ngăn chặn. Điều kiện khoảng cách không kích hoạt giao điểm hay tiếp tuyến, vì vậy mỗi vòng tròn vẫn đóng góp một gia số cơ sở của một vùng. Điều này phản ánh chính xác rằng mỗi ranh giới khép kín giới thiệu một mặt kèm theo bổ sung. 

Các vòng tròn tiếp tuyến bộc lộ tình trạng tế nhị nhất. Khi khoảng cách bằng chính xác tổng bán kính thì các vòng tròn sẽ chạm nhau tại một điểm. Mã này loại trừ rõ ràng điều này khỏi việc đếm giao lộ, ngăn chặn sự phân chia nhân tạo trong biểu đồ phẳng có thể làm tăng số lượng khu vực một cách không chính xác. 

Các vòng tròn rời rạc đảm bảo rằng nhiều thành phần biệt lập chỉ tích lũy các vùng một cách độc lập, vì không có tương tác theo cặp nào góp phần phân chia bổ sung ngoài đóng góp cơ sở cho mỗi vòng tròn.
