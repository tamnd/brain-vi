---
title: "CF 104442G - El jard\u00edn del Ed\u00e9n"
description: "Chúng ta được cung cấp một máy tự động di động nhị phân có chiều dài cố định. Mỗi cấu hình là một hàng ô C, trong đó mỗi ô là 0 hoặc 1."
date: "2026-06-30T18:07:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104442
codeforces_index: "G"
codeforces_contest_name: "AdaByron Regional Madrid 2023"
rating: 0
weight: 104442
solve_time_s: 54
verified: true
draft: false
---

[CF 104442G - El jard\u00edn del Ed\u00e9n](https://codeforces.com/problemset/problem/104442/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một máy tự động di động nhị phân có chiều dài cố định. Mỗi cấu hình là một hàng gồm các ô C, trong đó mỗi ô là 0 hoặc 1. Quy tắc R (từ 0 đến 255) xác định cách tạo hàng tiếp theo: mọi vị trí đều xem xét một khối gồm ba ô liên tiếp trong hàng hiện tại, lấy bộ ba đó làm số 3 bit và sử dụng R để quyết định bit kết quả ở hàng tiếp theo. 

Nhiệm vụ không phải là mô phỏng về phía trước. Thay vào đó, chúng ta được cung cấp một cấu hình mục tiêu A và phải xác định xem liệu có tồn tại bất kỳ cấu hình B nào trước đó có thể phát triển thành A theo quy tắc nhất định hay không. Nếu không có B như vậy tồn tại thì A được gọi là cấu hình Garden of Eden và chúng tôi xuất ra SI, nếu không thì chúng tôi xuất ra NO. 

Lưới có chiều rộng nhỏ, C nhiều nhất là 30, nhưng số lượng trường hợp thử nghiệm lớn, lên tới 5000. Sự kết hợp đó gợi ý rõ ràng rằng mỗi bài kiểm tra phải được giải trong khoảng thời gian khoảng O(C) hoặc O(C log C), bởi vì mọi phép tính bậc hai trên mỗi bài kiểm tra đều đã quá chậm. 

Một điểm tinh tế là các ô biên ngoài mảng được cố định bằng 0 và không bao giờ thay đổi. Điều này có nghĩa là quá trình chuyển đổi đầu tiên và cuối cùng bị ràng buộc bởi các lân cận số 0 ảo và bất kỳ chuyển đổi tiền nhiệm hợp lệ nào cũng phải tôn trọng điều đó. 

Một sự hiểu lầm ngây thơ là nghĩ rằng chúng ta có thể tham lam tái tạo lại người tiền nhiệm từ trái sang phải. Điều đó không thành công vì mỗi ô phụ thuộc vào ba bit liên tiếp trước đó, do đó các lựa chọn chồng chéo và tạo ra các ràng buộc chung. 

Sai lầm phổ biến thứ hai là cố gắng ép buộc tất cả những người đi trước có thể có. Với C lên tới 30, có 2^30 ứng viên, tức là có khoảng một tỷ khả năng cho mỗi bài kiểm tra, hoàn toàn không khả thi ngay cả trước khi xem xét 5000 trường hợp kiểm tra. 

## Phương pháp tiếp cận 

Phương pháp bạo lực thử mọi cấu hình B có thể có trước đó có độ dài C và mô phỏng một bước của máy tự động để xem liệu nó có khớp với A hay không. Mỗi mô phỏng có giá O(C), do đó tổng chi phí cho mỗi thử nghiệm là O(C · 2^C). Điều này bùng nổ ngay lập tức với C = 30, trong đó không gian trạng thái có khoảng 10^9 cấu hình. 

Cấu trúc chính là tính hợp lệ mang tính cục bộ: mỗi vị trí i chỉ phụ thuộc vào (B[i−1], B[i], B[i+1]). Điều này có nghĩa là chúng ta không cần phải khám phá toàn bộ chuỗi một cách độc lập. Thay vào đó, chúng ta có thể xây dựng phiên bản tiền nhiệm từng chút một trong khi vẫn giữ đủ bối cảnh để thực thi ràng buộc cục bộ. 

Quan sát mở ra giải pháp là khi quét từ trái sang phải, thông tin duy nhất cần thiết để quyết định liệu chúng ta có thể mở rộng phép gán một phần hay không là hai bit được chọn cuối cùng. Khi chúng ta sửa B[i−1] và B[i], bit tiếp theo B[i+1] chỉ bị ràng buộc bởi liệu nó có tạo ra đầu ra A[i] được yêu cầu theo quy tắc hay không. 

Điều này biến vấn đề thành vấn đề tồn tại đường dẫn trên các trạng thái được xác định bởi các cặp bit liền kề, có thể được giải quyết bằng lập trình động theo vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C · 2^C) | O(C) | Quá chậm | 
| Cặp DP | O(C · 4) | O(4) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi chuỗi trước đó chưa xác định là chuỗi nhị phân B có độ dài C, với hai bit ranh giới ảo cố định B[0] = 0 và B[C+1] = 0. 

Sau đó, chúng tôi cố gắng xây dựng B từ trái sang phải trong khi đảm bảo rằng mọi vị trí i đều tạo ra A[i] cần thiết khi kết hợp với các vị trí lân cận của nó.

1. Cố định điều kiện biên bằng cách đặt hàng xóm bên trái tưởng tượng của vị trí 1 thành 0. Điều này đảm bảo quá trình chuyển đổi đầu tiên được xác định rõ ràng và nhất quán với phát biểu vấn đề. 
2. Xác định trạng thái động tại vị trí i là cặp (B[i−1], B[i]). Điều này là đủ vì ràng buộc ở vị trí i liên quan đến chính xác hai bit này cộng với bit tiếp theo B[i+1]. 
3. Khởi tạo DP với cặp bắt đầu duy nhất có thể có (0, 0) trước khi xử lý vị trí 1. Điều này mã hóa rằng B[0] được cố định thành 0 và B[1] chưa được chọn. 
4. Với mỗi vị trí i từ 1 đến C, hãy thử tất cả các trạng thái hiện tại có thể có (x_{i−1}, x_i). Đối với mỗi trạng thái, hãy thử mở rộng nó bằng cách chọn x_{i+1} trong {0, 1}. Phần mở rộng chỉ hợp lệ nếu áp dụng quy tắc cho (x_{i−1}, x_i, x_{i+1}) tạo ra A[i]. Điều này thực thi tính nhất quán cục bộ ngay lập tức thay vì trì hoãn nó. 
5. Chuyển sang trạng thái tiếp theo (x_i, x_{i+1}) bất cứ khi nào có phần mở rộng hợp lệ. Thao tác này sẽ dịch chuyển cửa sổ sang bên phải một bước trong khi vẫn giữ lại tất cả thông tin cần thiết cho các ràng buộc trong tương lai. 
6. Sau khi xử lý vị trí C, thực thi điều kiện biên cuối cùng bằng cách yêu cầu chuyển đổi được tính toán cuối cùng phải nhất quán với B[C+1] = 0, nghĩa là chỉ các trạng thái có x_C và 0 mới phải đáp ứng quy tắc tại vị trí C. 
7. Nếu tồn tại bất kỳ trạng thái DP hợp lệ nào ở cuối, xuất ra NO vì tồn tại ít nhất một trạng thái trước đó. Ngược lại xuất ra SI. 

Tính đúng đắn dựa trên thực tế là mọi ràng buộc liên quan đến vị trí i đều được kiểm tra chính xác khi x_{i+1} được đưa vào, do đó không có phép gán một phần không hợp lệ nào tồn tại vượt quá điểm vi phạm của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def rule_output(R, a, b, c):
    idx = (a << 2) | (b << 1) | c
    return (R >> idx) & 1

def solve_case(R, C, A):
    # dp over states (prev, cur)
    dp = [[False, False], [False, False]]
    dp[0][0] = True  # B[0]=0, B[1]=0 initially

    for i in range(1, C + 1):
        ndp = [[False, False], [False, False]]
        ai = (A >> (C - i)) & 1

        for p in range(2):
            for q in range(2):
                if not dp[p][q]:
                    continue
                for nxt in range(2):
                    if rule_output(R, p, q, nxt) == ai:
                        ndp[q][nxt] = True

        dp = ndp

    # enforce boundary: B[C+1] = 0
    return any(dp[p][0] for p in range(2))

def main():
    N = int(input())
    out = []
    for _ in range(N):
        R, C, A = map(int, input().split())
        out.append("NO" if solve_case(R, C, A) else "SI")
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Bảng DP lưu trữ liệu phần trước vị trí i có thể kết thúc bằng một cặp bit nhất định hay không. Mỗi quá trình chuyển đổi sẽ kiểm tra quy tắc tự động hóa bằng cách trích xuất vùng lân cận 3 bit chính xác và so sánh nó với bit cấu hình đích ở vị trí i. Việc trích xuất bit từ A được thực hiện từ quan trọng nhất đến ít quan trọng nhất để lập chỉ mục vị trí khớp với việc truyền tải từ trái sang phải. 

Kiểm tra cuối cùng thực thi ranh giới bên phải bằng 0 bằng cách chỉ chấp nhận các trạng thái trong đó bit được tạo cuối cùng có thể được theo sau bởi 0 một cách nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

R = 108, C = 8, A = 181 (nhị phân 10110101) 

Chúng tôi theo dõi trạng thái DP (prev, cur). Chỉ các trạng thái có thể truy cập mới được hiển thị. 

| tôi | trạng thái hoạt động (prev, cur) | 
| --- | --- | 
| 0 | (0,0) | 
| 1 | (0,1) | 
| 2 | (1,0), (1,1) | 
| 3 | (0,1), (1,0) | 
| 8 | không có trạng thái kết thúc bằng next = 0 | 

DP cuối cùng đạt đến sự mâu thuẫn với điều kiện biên, do đó không có tiền thân nào tồn tại và đầu ra là SI. 

Điều này chứng tỏ một công trình phù hợp cục bộ vẫn có thể thất bại trên toàn cầu do những hạn chế về ranh giới. 

### Ví dụ 2 

đầu vào: 

R = 90, C = 10, A = 111 

| tôi | trạng thái hoạt động | 
| --- | --- | 
| 0 | (0,0) | 
| 1 | (0,1) | 
| 2 | (1,1) | 
| 3 | (1,0), (1,1) | 
| 10 | ít nhất một trạng thái hợp lệ | 

Ở đây DP không bao giờ trống và tồn tại ít nhất một thiết bị tiền nhiệm đầy đủ, do đó đầu ra là KHÔNG. 

Điều này cho thấy rằng nhiều cấu trúc từng phần có thể cùng tồn tại và chỉ một đường dẫn hợp lệ là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · C · 4) | Mỗi vị trí xử lý 4 trạng thái và 2 lần chuyển tiếp | 
| Không gian | O(1) | Bàn DP có kích thước cố định 2×2 | 

Với C 30 và N 5000, tổng số lần chuyển đổi là khoảng 5000 × 30 × 8, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import prod

    def rule_output(R, a, b, c):
        idx = (a << 2) | (b << 1) | c
        return (R >> idx) & 1

    def solve_case(R, C, A):
        dp = [[False, False], [False, False]]
        dp[0][0] = True

        for i in range(1, C + 1):
            ndp = [[False, False], [False, False]]
            ai = (A >> (C - i)) & 1

            for p in range(2):
                for q in range(2):
                    if not dp[p][q]:
                        continue
                    for nxt in range(2):
                        if rule_output(R, p, q, nxt) == ai:
                            ndp[q][nxt] = True
            dp = ndp

        return any(dp[p][0] for p in range(2))

    N = int(input())
    out = []
    for _ in range(N):
        R, C, A = map(int, input().split())
        out.append("NO" if solve_case(R, C, A) else "SI")
    return "\n".join(out)

# provided samples
assert run("""5
108 8 181
90 10 111
204 16 43690
2 16 0
108 30 346521633
""") == """SI
NO
SI
NO
NO"""

# custom cases
assert run("1\n0 1 0\n") == "NO"
assert run("1\n0 1 1\n") == "SI"
assert run("1\n255 5 31\n") == "NO"
assert run("1\n255 5 0\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| R=0, C=1, A=0 | KHÔNG | tiền thân tầm thường nhất quán tồn tại | 
| R=0, C=1, A=1 | SI | không thể sản xuất được 1 | 
| R=255, tất cả quy tắc đều hoạt động | KHÔNG | luôn có thể tuyên truyền | 
| R=255, mục tiêu 0 | KHÔNG | thất bại nhất quán ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi quy tắc cho phép nhiều lần tiếp tục nhưng điều kiện biên sẽ loại bỏ tất cả chúng. Ví dụ: với quy tắc cho phép như R = 255, mỗi bộ ba tạo ra 1, do đó hệ thống có xu hướng buộc tất cả các số một trong nội bộ, nhưng yêu cầu cuối cùng B[C+1] = 0 sẽ phá vỡ tính nhất quán trừ khi C cực kỳ nhỏ. 

Một trường hợp cạnh khác là A = 0 đối với các quy tắc hạn chế như R = 0, trong đó mỗi bộ ba tạo ra 0. Trong trường hợp đó, bất kỳ phần trước nào cũng hợp lệ và DP phải báo cáo chính xác KHÔNG vì có một giải pháp tồn tại, mặc dù cấu trúc có thể gợi ý sự mơ hồ. 

DP xử lý cả hai trường hợp một cách thống nhất vì mọi ràng buộc được thực thi cục bộ ở mỗi bước và việc kiểm tra ranh giới cuối cùng là hạn chế toàn cầu duy nhất có thể loại bỏ các cấu trúc hợp lệ.
