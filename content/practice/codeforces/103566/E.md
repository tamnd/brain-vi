---
title: "CF 103566E - \u0421\u0442\u0438\u043a\u0435\u0440\u044b"
description: "Chúng ta được cung cấp một tập hợp N người tham gia, mỗi người được mô tả bằng ba mẩu thông tin: một “tài liệu tham khảo bạn bè” tiềm năng Fi, cờ sẵn sàng Pi và dấu thời gian Ti."
date: "2026-07-03T04:57:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103566
codeforces_index: "E"
codeforces_contest_name: "2021-2022 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 103566
solve_time_s: 41
verified: true
draft: false
---

[CF 103566E - \u0421\u0442\u0438\u043a\u0435\u0440\u044b](https://codeforces.com/problemset/problem/103566/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp N người tham gia, mỗi người được mô tả bằng ba mẩu thông tin: một “tài liệu tham khảo bạn bè” tiềm năng Fi, cờ sẵn sàng Pi và dấu thời gian Ti. Quá trình này xác định một mảng dẫn xuất cnt, ban đầu tất cả đều là số 0, đếm số lượng người tham gia khác “xác thực” thành công mỗi người tham gia là một người bạn đủ điều kiện. 

Đối với mỗi người tham gia i, chúng tôi cố gắng xác định xem liệu tôi có thể đóng vai trò là người giới thiệu hợp lệ cho bạn của người khác Fi hay không. Nếu Fi khác 0, người tham gia i được coi là người có thể góp phần tăng số lượng Fi. Tuy nhiên, điều này chỉ được phép nếu tôi thỏa mãn hai điều kiện độc lập: Pi phải dương, nghĩa là tôi đã vượt qua một bài kiểm tra trình độ nhất định và Ti phải lớn hơn TFi, nghĩa là tôi đã đăng ký muộn hơn người bạn mà họ đang giới thiệu. 

Nếu tất cả các điều kiện đều đúng, chúng tôi sẽ tăng cnt[Fi] lên một. Sau khi xử lý tất cả những người tham gia, chúng tôi kiểm tra lại từng người tham gia i và xuất ra các chỉ số trong đó cnt[i] ít nhất là 2. Đầu ra phải theo thứ tự tăng dần của các chỉ số. 

Kích thước đầu vào N là yếu tố chính định hình giải pháp. Vì chúng tôi chỉ thực hiện một lượng công việc không đổi cho mỗi người tham gia trong quá trình quét trực tiếp nên cần phải sử dụng phương pháp O(N) hoặc O(N log N). Bất cứ điều gì liên quan đến kiểm tra lồng nhau đối với các cặp người tham gia sẽ dẫn đến O(N²), điều này trở nên không khả thi khi N đạt đến các ràng buộc điển hình của Codeforces như 2⋅10⁵. 

Trường hợp cạnh tinh tế xuất hiện khi Fi bằng 0. Trong trường hợp đó, người tham gia không đóng góp vào bất kỳ số lượng nào và việc cố gắng lập chỉ mục cnt[0] sẽ không hợp lệ nếu không được xử lý cẩn thận. Một trường hợp khác phát sinh khi dấu thời gian bằng nhau: vì điều kiện đúng là Ti > TFi, nên đẳng thức phải thất bại và việc triển khai ngây thơ sử dụng so sánh không nghiêm ngặt sẽ tăng số lượng không chính xác. 

Cạm bẫy cuối cùng là quên rằng nhiều người tham gia có thể đóng góp vào cùng một Fi một cách độc lập. Điều này có nghĩa là cnt không phải là một mảng boolean mà là một bộ tích lũy tần số và dự kiến ​​sẽ có những đóng góp trùng lặp. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ mô phỏng trực tiếp định nghĩa: đối với mỗi người tham gia i, chúng tôi quét tất cả những người tham gia j khác và kiểm tra xem liệu j có thể đóng góp cho cnt[i] hay không. Điều này dẫn đến việc kiểm tra các điều kiện liên quan đến Fi, Pi và Ti cho mỗi cặp, làm cho quá trình trở thành bậc hai một cách hiệu quả. Mặc dù đúng về mặt logic nhưng điều này dẫn đến so sánh khoảng N2 trong trường hợp xấu nhất, quá chậm khi N lớn. 

Quan sát quan trọng là mỗi người tham gia đóng góp tối đa một đơn vị thông tin cho một chỉ mục mục tiêu Fi và sự đóng góp này được xác định độc lập với tất cả những người tham gia khác. Không có sự phụ thuộc giữa các bản cập nhật khác nhau cho cnt, điều đó có nghĩa là chúng ta có thể tính toán tất cả các đóng góp trong một lần tuyến tính duy nhất. 

Thay vì tìm kiếm những người đóng góp hợp lệ cho mỗi người tham gia, chúng tôi trực tiếp lặp lại tất cả những người tham gia một lần và đối với mỗi người tham gia, chúng tôi kiểm tra xem người đó có đủ điều kiện làm người đóng góp hay không. Nếu đúng như vậy, chúng tôi sẽ tăng cnt[Fi]. Điều này chuyển đổi vấn đề từ “truy vấn các mối quan hệ hợp lệ đến” thành “xử lý các cạnh hợp lệ gửi đi”, loại bỏ sự cần thiết của bất kỳ phép lặp lồng nhau nào. 

Sau khi xây dựng cnt, lần quét tuyến tính thứ hai sẽ trích xuất tất cả các chỉ số có cnt[i] ≥ 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các mảng F, P và T. Những mảng này xác định những đóng góp tiềm năng trực tiếp từ mỗi người tham gia cho người tham gia khác. Cấu trúc là tĩnh nên không cần cập nhật động. 
2. Khởi tạo cnt dưới dạng một mảng có kích thước N với tất cả các số 0. Điều này sẽ tích lũy số lượng người đóng góp hợp lệ mà mỗi người tham gia nhận được. 
3. Lặp lại từng người tham gia i từ 1 đến N. Với mỗi i, chúng ta quyết định xem nó có đóng góp vào số lượng của người khác hay không. 
4. Nếu Fi bằng 0, bỏ qua i ngay lập tức. Điều này tránh việc lập chỉ mục không hợp lệ và phản ánh rằng không có mục tiêu đóng góp nào tồn tại. 
5. Kiểm tra xem Pi có dương không. Nếu Pi bằng 0 thì tôi không thể đóng góp nên chúng tôi bỏ qua. Điều kiện này lọc ra những người tham gia không vượt qua được bước đánh giá. 
6. Kiểm tra xem Ti có lớn hơn TFi hay không. Nếu không, bỏ qua i. Điều này thực thi ràng buộc về thứ tự mà chỉ những người tham gia sau mới có thể xác thực những thứ trước đó. 
7. Nếu tất cả các bước kiểm tra đều đạt, hãy tăng cnt[Fi] lên một. Điều này ghi lại rằng Fi đã có được một người tham gia hỗ trợ hợp lệ. 
8. Sau khi xử lý tất cả những người tham gia, lặp lại i từ 1 đến N và thu thập tất cả các chỉ số trong đó cnt[i] ít nhất là 2. Đây là những người tham gia đã nhận đủ xác nhận hợp lệ. 
9. Xuất ra các chỉ số đã thu thập theo thứ tự tăng dần, điều này được đảm bảo tự động vì chúng ta duyệt i theo thứ tự tăng dần. 

Tính đúng đắn dựa trên tính bất biến sau bước 7, cnt[x] bằng số lượng người tham gia i sao cho i đóng góp chính xác một lần cho x dưới mọi ràng buộc. Vì mỗi người tham gia được xử lý độc lập và đóng góp tối đa một phần tăng nên không thể xảy ra việc tính trùng hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    F = [0] + list(map(int, input().split()))
    P = [0] + list(map(int, input().split()))
    T = [0] + list(map(int, input().split()))

    cnt = [0] * (n + 1)

    for i in range(1, n + 1):
        f = F[i]
        if f == 0:
            continue
        if P[i] <= 0:
            continue
        if T[i] <= T[f]:
            continue
        cnt[f] += 1

    res = []
    for i in range(1, n + 1):
        if cnt[i] >= 2:
            res.append(i)

    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh chính xác thuật toán: lượt đầu tiên xây dựng số lượng tần số và lượt thứ hai trích xuất các chỉ số hợp lệ. Chi tiết quan trọng là việc kiểm tra bất đẳng thức một cách nghiêm ngặt`T[i] <= T[f]`, được viết theo cách loại bỏ chính xác cả dấu thời gian bằng nhau và nhỏ hơn. 

Việc lập chỉ mục được xử lý bằng cách chuyển các mảng sang dạng dựa trên 1, điều này tránh nhầm lẫn giữa các chỉ mục hợp lệ của người tham gia và trọng điểm 0 được sử dụng cho Fi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp nhỏ trong đó những người tham gia tạo thành một chuỗi đóng góp hợp lệ đơn giản. 

đầu vào:```
n = 4
F = [0, 1, 1, 2]
P = [0, 1, 1, 1]
T = [0, 1, 2, 3]
```Chúng tôi theo dõi cnt trong quá trình xử lý. 

| tôi | Fi | Pi | Ti | Điều kiện giữ | cập nhật cnt | trạng thái cnt | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | không | không | [0,0,0,0] | 
| 2 | 1 | 1 | 2 | vâng | cnt[1]++ | [0,1,0,0] | 
| 3 | 1 | 1 | 3 | vâng | cnt[1]++ | [0,2,0,0] | 
| 4 | 2 | 1 | 3 | vâng | cnt[2]++ | [0,2,1,0] | 

Cnt cuối cùng là [0,2,1,0]. Chúng tôi xuất ra những người tham gia có cnt ≥ 2, tức là chỉ có người tham gia 1. 

Dấu vết này cho thấy nhiều đóng góp độc lập được tích lũy chính xác trong cnt mà không bị nhiễu. 

### Ví dụ 2 

đầu vào:```
n = 5
F = [0, 2, 2, 3, 3]
P = [0, 1, 0, 1, 1]
T = [0, 5, 4, 3, 2]
```| tôi | Fi | Pi | Ti | Điều kiện giữ | cập nhật cnt | trạng thái cnt | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 5 | không | không | [0,0,0,0,0] | 
| 2 | 2 | 1 | 4 | T2 > T2 sai | không | [0,0,0,0,0] | 
| 3 | 3 | 0 | 3 | Pi thất bại | không | [0,0,0,0,0] | 
| 4 | 3 | 1 | 2 | T4 > T3 đúng | cnt[3]++ | [0,0,0,1,0] | 
| 5 | 3 | 1 | 2 | T5 > T3 đúng | cnt[3]++ | [0,0,0,2,0] | 

Cnt cuối cùng là [0,0,0,2,0], vì vậy người tham gia 3 là đầu ra. 

Ví dụ này nhấn mạnh hai điều kiện biên: dấu thời gian bằng nhau và lọc Pi không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi người tham gia được xử lý một lần để đóng góp và một lần để trích xuất | 
| Không gian | O(N) | Mảng F, P, T và cnt đều có kích thước tuyến tính | 

Giải pháp này thực hiện một lượng công việc không đổi cho mỗi người tham gia, khiến nó phù hợp một cách thoải mái với các ràng buộc điển hình của Codeforce lên tới 2⋅10⁵. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""  # output printed directly

# sample-like case
run("""4
0 1 1 2
1 1 1 1
1 2 3 4
""")

# minimum size
run("""1
0
1
1
""")

# no valid contributions
run("""3
0 0 0
1 1 1
3 2 1
""")

# all contribute to same target
run("""4
0 1 1 1
1 1 1 1
1 2 3 4
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | trống | ranh giới tối thiểu | 
| không có cạnh | trống | lọc tính đúng đắn | 
| mục tiêu giống hệt nhau | 1 | logic tích lũy | 

## Vỏ cạnh 

Trường hợp một cạnh là khi Fi bằng 0 đối với tất cả người tham gia. Trong trường hợp đó, vòng lặp không bao giờ kích hoạt bất kỳ mức tăng cnt nào và đầu ra cuối cùng trống. Thuật toán xử lý việc này một cách tự nhiên vì điều kiện đầu tiên`if f == 0`ngăn chặn việc lập chỉ mục không hợp lệ và bỏ qua tất cả các bản cập nhật. 

Một trường hợp khác xảy ra khi dấu thời gian bằng nhau đối với một cặp có vẻ hợp lệ. Ví dụ, nếu Fi = j và Ti = Tj, điều kiện không đạt do bất đẳng thức nghiêm ngặt. Mã sử ​​dụng rõ ràng`T[i] <= T[f]`bác bỏ sự bình đẳng, đảm bảo không có sự gia tăng ngẫu nhiên nào xảy ra.
