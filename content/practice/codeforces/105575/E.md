---
title: "CF 105575E - \u5b89\u6392\u65f6\u95f4"
description: "Chúng ta được giao một bài toán lập kế hoạch tiến độ. Có một số người và mỗi người cung cấp một hoặc nhiều khoảng thời gian mà họ có mặt. Mục tiêu là xác định thời điểm có số lượng người tối đa có mặt đồng thời."
date: "2026-06-22T20:39:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105575
codeforces_index: "E"
codeforces_contest_name: "Southeast University 6th Programming Competition Competition (Winter, Novice Group) \u4e1c\u5357\u5927\u5b66\u7b2c\u516d\u5c4a\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u51ac\u5b63\u8d5b\uff08\u65b0\u624b\u7ec4\uff09"
rating: 0
weight: 105575
solve_time_s: 48
verified: true
draft: false
---

[CF 105575E - \u5b89\u6392\u65f6\u95f4](https://codeforces.com/problemset/problem/105575/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một bài toán lập kế hoạch tiến độ. Có một số người và mỗi người cung cấp một hoặc nhiều khoảng thời gian mà họ có mặt. Mục tiêu là xác định thời điểm có số lượng người tối đa có mặt đồng thời. Sau khi tìm thấy thời điểm tốt nhất đó, chúng tôi cũng báo cáo số lượng người không có mặt tại thời điểm đó. 

Cụ thể hơn, thời gian được rời rạc hóa thành các vị trí nguyên từ 1 đến m. Mỗi người đóng góp một số khoảng thời gian khép kín và mỗi khoảng thời gian cho biết rằng người đó sẵn sàng cho tất cả các thời điểm trong đó. Một người được coi là sẵn sàng tại một thời điểm nếu ít nhất một trong các khoảng thời gian của họ bao gồm nó, nhưng trong công thức bài toán này, mỗi khoảng thời gian được coi trực tiếp là tính sẵn sàng đóng góp trong phạm vi của nó, vì vậy chúng tôi đếm một cách hiệu quả có bao nhiêu khoảng thời gian bao phủ trùng lặp tại mỗi thời điểm. 

Đầu ra yêu cầu hai điều: thời điểm mà số lượng người sẵn sàng được tối đa hóa và số người không có mặt tại thời điểm đó. 

Các ràng buộc ngụ ý rằng n có thể lớn và mỗi người có thể có nhiều khoảng thời gian, do đó, cách tiếp cận đơn giản kiểm tra mọi điểm thời gian đối với mọi khoảng thời gian sẽ nhanh chóng trở nên quá chậm. Nếu chúng tôi giả định tối đa 10^5 điểm thời gian và nhiều khoảng thời gian, thì mô phỏng trực tiếp sẽ dẫn đến hành vi gần như O(n · m) trong trường hợp xấu nhất, điều này không khả thi trong giới hạn thông thường. 

Một vấn đề tinh tế hơn xuất hiện với việc xử lý ranh giới. Các khoảng đều mang tính bao hàm, vì vậy nếu một khoảng kết thúc tại r, chúng ta phải đảm bảo rằng nó đóng góp vào thời gian r chứ không đóng góp vào r + 1. Việc triển khai tiền tố ngây thơ mà quên chi tiết sai lệch này sẽ dịch chuyển số đếm về phía trước một cách không chính xác. 

Một trường hợp cạnh đơn giản cho thấy rõ điều này. Giả sử m = 5 và một khoảng duy nhất là [2, 3]. Tại thời điểm thứ 3, người đó vẫn được tính. Nếu chúng ta giảm nhầm ở vị trí 3 thay vì 4, chúng ta sẽ mất phạm vi bao phủ ở thời điểm thứ 3 và đếm thiếu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: với mỗi thời điểm từ 1 đến m, hãy kiểm tra từng khoảng thời gian của mỗi người và đếm xem có bao nhiêu khoảng thời gian bao gồm thời gian đó. Điều này đúng vì nó mô phỏng trực tiếp định nghĩa về tính sẵn có. Tuy nhiên, nếu có tối đa n người và mỗi người có p khoảng thời gian và m điểm thời gian, thì giá trị này sẽ trở thành O(m · tổng_khoảng), có thể giảm xuống còn khoảng 10^10 thao tác trong trường hợp xấu nhất. 

Quan sát quan trọng là chúng ta không thực sự cần phải tính toán lại mức độ phù hợp một cách độc lập cho mọi thời điểm. Mỗi khoảng thời gian đóng góp một khối phủ sóng +1 liên tục. Thay vì áp dụng đóng góp này nhiều lần trong mỗi khoảng thời gian, chúng ta có thể mã hóa hiệu ứng của nó bằng cách sử dụng một mảng hiệu: tăng ở l và giảm ở r + 1. Sau khi xử lý tất cả các khoảng, tổng tiền tố sẽ tái tạo lại số khoảng hoạt động tại mỗi thời điểm. 

Điều này biến đổi vấn đề từ việc liên tục thêm các phạm vi thành một lần quét tuyến tính duy nhất. Khi chúng tôi có vùng phủ sóng ở mọi thời điểm, chúng tôi có thể quét một lần để tìm vị trí tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · tổng các khoảng) | O(1) | Quá chậm | 
| Mảng chênh lệch + Tổng tiền tố | O(m + tổng khoảng) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng một mảng khác biệt trên trục thời gian. 

1. Tạo một mảng`diff`có kích thước m + 2 được khởi tạo bằng 0. Mảng này sẽ lưu trữ cách bắt đầu và dừng đóng góp theo khoảng thời gian. Chúng tôi sử dụng m + 2 để xử lý chỉ số r + 1 một cách an toàn mà không gặp vấn đề về giới hạn. 
2. Đối với mỗi người, hãy đọc khoảng thời gian của họ. Với mỗi khoảng [l, r], tăng dần`diff[l]`bằng 1 và giảm dần`diff[r + 1]`bằng 1. Điều này mã hóa rằng khoảng đóng góp +1 bắt đầu từ l và dừng đóng góp sau r. 
3. Sau khi xử lý tất cả các khoảng, hãy tính tổng tiền tố`diff`để có được`cnt[i]`, số khoảng bao phủ thời gian i. Mỗi vị trí tích lũy tất cả các đóng góp khoảng thời gian hoạt động bao gồm nó. 
4. Duyệt qua tất cả các điểm thời gian từ 1 đến m và theo dõi chỉ số nơi`cnt[i]`được tối đa hóa. Điều này thể hiện thời điểm mà số lượng khoảng thời gian chồng lên nhau lớn nhất. 
5. Đưa ra thời gian tốt nhất và số người không có mặt tại thời điểm đó, được tính bằng n trừ đi giá trị bảo hiểm tối đa. 

### Tại sao nó hoạt động 

Mỗi khoảng [l, r] có thể được xem như là phép cộng +1 cho mọi vị trí trong một đoạn liền kề. Mảng chênh lệch mã hóa bản cập nhật phân đoạn này chính xác một lần trong mỗi khoảng thời gian và tổng tiền tố sẽ tái tạo lại giá trị tích lũy chính xác tại mỗi điểm. Bởi vì phép cộng là tuyến tính và độc lập giữa các khoảng, nên tổng tiền tố cuối cùng tương đương với việc áp dụng rõ ràng tất cả các đóng góp khoảng. Do đó, giá trị tối đa được xây dựng lại là mức chồng lấp tối đa toàn cầu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    diff = [0] * (m + 2)

    for _ in range(n):
        p = int(input())
        for _ in range(p):
            l, r = map(int, input().split())
            diff[l] += 1
            if r + 1 <= m:
                diff[r + 1] -= 1

    best_time = 1
    best_val = -1
    cur = 0

    for i in range(1, m + 1):
        cur += diff[i]
        if cur > best_val:
            best_val = cur
            best_time = i

    print(best_time, n - best_val)

if __name__ == "__main__":
    solve()
```Mã trực tiếp phản ánh ý tưởng mảng khác biệt. các`diff`mảng chỉ ghi lại các hiệu ứng biên của các khoảng. Biến`cur`duy trì tổng tiền tố đang chạy, là số khoảng thời gian hoạt động thực tế tại mỗi thời điểm. Bước so sánh cập nhật thời điểm tốt nhất bất cứ khi nào tìm thấy mức trùng lặp cao hơn. Phép trừ cuối cùng`n - best_val`chuyển đổi số người tham gia tối đa thành số người vắng mặt. 

Một chi tiết triển khai tinh tế là điều kiện`if r + 1 <= m`. Vì chúng ta chỉ quan tâm đến thời gian lên tới m nên bất kỳ mức giảm nào vượt quá phạm vi đó đều không liên quan, nhưng việc giữ an toàn cho mảng sẽ tránh việc vô tình ghi ra ngoài giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
1
1 2
1
2 4
1
3 5
```Chúng tôi xử lý sự khác biệt: 

| Khoảng thời gian | thay đổi khác biệt | 
| --- | --- | 
| [1,2] | +1 lúc 1, -1 lúc 3 | 
| [2,4] | +1 lúc 2, -1 lúc 5 | 
| [3,5] | +1 lúc 3, -1 lúc 6 | 

Tích lũy tiền tố: 

| tôi | khác biệt [i] | cur | 
| --- | --- | --- | 
| 1 | +1 | 1 | 
| 2 | +1 | 2 | 
| 3 | 0 | 2 | 
| 4 | 0 | 2 | 
| 5 | -1 | 1 | 

Tối đa là 2 lúc 2-4, sớm nhất là 2. 

Đầu ra:```
2 1
```Điều này cho thấy các khoảng chồng chéo được tích lũy một cách chính xác ngay cả khi chúng đến từ những người khác nhau. 

### Ví dụ 2 

đầu vào:```
2 4
2
1 2
3 4
1
2 3
```| Khoảng thời gian | thay đổi khác biệt | 
| --- | --- | 
| [1,2] | +1 lúc 1, -1 lúc 3 | 
| [3,4] | +1 lúc 3, -1 lúc 5 | 
| [2,3] | +1 lúc 2, -1 lúc 4 | 

Tiền tố: 

| tôi | cur | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 1 | 

Tối đa là 2 ở lần 2 và 3 nên ta chọn 2. 

Đầu ra:```
2 0
```Điều này thể hiện việc xử lý chính xác các ranh giới chồng chéo trong đó nhiều khoảng giao nhau chính xác tại các điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + tổng khoảng) | Mỗi khoảng thời gian được xử lý một lần và một lần quét tuyến tính theo thời gian được thực hiện | 
| Không gian | O(m) | Tích lũy mảng và tiền tố khác nhau trong phạm vi thời gian | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì cả số lượng khoảng thời gian và phạm vi thời gian đều được xử lý theo thời gian tuyến tính, tránh bất kỳ lần lặp lồng nhau nào giữa chúng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    diff = [0] * (m + 2)

    for _ in range(n):
        p = int(input())
        for _ in range(p):
            l, r = map(int, input().split())
            diff[l] += 1
            diff[r + 1] -= 1

    best_time = 1
    best_val = -1
    cur = 0

    for i in range(1, m + 1):
        cur += diff[i]
        if cur > best_val:
            best_val = cur
            best_time = i

    return f"{best_time} {n - best_val}\n"

# provided sample-like case
assert run("3 5\n1\n1 2\n1\n2 4\n1\n3 5\n") == "2 1\n"

# minimum size
assert run("1 1\n1\n1 1\n") == "1 0\n"

# non-overlapping intervals
assert run("2 5\n1\n1 1\n1\n5 5\n") == "1 1\n"

# full overlap
assert run("3 3\n1\n1 3\n1\n1 3\n1\n1 3\n") == "1 0\n"

# boundary test
assert run("2 4\n1\n1 2\n1\n2 3\n") == "2 0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng thời gian tối thiểu | 1 0 | trường hợp biên nhỏ nhất | 
| khoảng rời rạc | 1 1 | đúng đắn khi không trùng lặp | 
| chồng chéo hoàn toàn | 1 0 | hành vi xếp chồng tối đa | 
| khoảng chạm ranh giới | 2 0 | xử lý đúng các chuyển tiếp r+1 | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các khoảng kết thúc chính xác tại m. Trong trường hợp đó, việc giảm ở r + 1 không được ảnh hưởng đến các chỉ số hợp lệ. Mã sẽ bỏ qua r + 1 một cách an toàn khi nó vượt quá m, do đó tiền tố cuối cùng vẫn phản ánh phạm vi bao phủ đầy đủ cho đến hết. 

Một trường hợp khác là khi các khoảng chỉ chạm nhau ở điểm cuối. Ví dụ: [1,2] và [2,3] đều phải đóng góp tại thời điểm 2. Mảng chênh lệch giữ chính xác cả hai hoạt động ở i = 2 vì mức giảm trong khoảng đầu tiên xảy ra ở 3 chứ không phải 2. Điều này duy trì sự chồng chéo ở ranh giới chính xác như yêu cầu. 

Trường hợp cạnh cuối cùng là khi mọi khoảng đều rời rạc. Thuật toán sẽ vẫn tính toán số lượng chính xác vì mỗi phân đoạn đóng góp độc lập và không bao giờ can thiệp vào các phân đoạn khác và mức tối đa tự nhiên trở thành 1 tại bất kỳ điểm nào được đề cập.
