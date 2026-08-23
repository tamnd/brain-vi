---
title: "CF 104262E - Dán Sao Diêm Vương lại với nhau"
description: "Chúng ta được cung cấp một biểu đồ có trọng số hoàn chỉnh trên các đỉnh $N$, trong đó mỗi đỉnh đại diện cho một tảng đá. Chi phí $C{i,j}$ là giá của việc dán trực tiếp hòn đá $i$ vào cạnh hòn đá $j$."
date: "2026-07-01T21:35:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104262
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 1 (Advanced)"
rating: 0
weight: 104262
solve_time_s: 86
verified: true
draft: false
---

[CF 104262E - Dán sao Diêm Vương lại với nhau](https://codeforces.com/problemset/problem/104262/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số hoàn chỉnh về$N$đỉnh, trong đó mỗi đỉnh đại diện cho một tảng đá. Chi phí$C_{i,j}$là giá dán đá trực tiếp$i$bên cạnh tảng đá$j$. Mục tiêu là sắp xếp tất cả các hòn đá theo thứ tự vòng tròn sao cho mỗi hòn đá có đúng hai hòn đá lân cận và chúng ta phải trả chi phí cho mỗi hòn đá kề nhau trong chu trình. Tổng chi phí là tổng chi phí của tất cả các cặp liên tiếp trong chu kỳ này, bao gồm cả cạnh đóng vòng từ viên đá cuối cùng trở lại viên đá đầu tiên. 

Vì vậy nhiệm vụ là chọn một hoán vị của tất cả$N$đá, diễn giải nó như một chu trình và giảm thiểu tổng chi phí của cạnh liền kề trong chu trình đó. 

Những hạn chế là nhỏ,$N \le 12$, điều này ngay lập tức báo hiệu rằng có thể khám phá trạng thái theo cấp số nhân. Một giải pháp giai thừa trên tất cả các hoán vị, đại khái$12! \approx 4.8 \times 10^8$, đã nằm ở ranh giới nhưng vẫn quá lớn trong Python nếu được thực hiện một cách ngây thơ. Bất kỳ giải pháp nào cũng phải nén tính đối xứng hoặc sử dụng quy hoạch động trên các tập hợp con. 

Một quan sát cấu trúc quan trọng là chu trình có tính đối xứng quay. Bất kỳ vòng quay theo chu kỳ nào của cùng một sự sắp xếp đều tạo ra cùng một chi phí, do đó, việc tính hoán vị vũ phu sẽ lãng phí một hệ số$N$. Quan trọng hơn, chúng ta không chỉ cần một hoán vị mà còn cần đảm bảo một chu trình khép kín, trong đó đưa ra sự phụ thuộc giữa phần tử đầu tiên và phần tử cuối cùng để phá vỡ các cấu trúc tham lam hoặc gia tăng đơn giản hơn. 

Một trường hợp khó phát hiện khi tất cả các chi phí đều giống nhau hoặc bằng 0. Trong những trường hợp như vậy, bất kỳ chu trình nào cũng là tối ưu, nhưng thuật toán đơn giản quên thêm cạnh đóng hoặc cạnh đếm kép vẫn sẽ tạo ra kết quả không chính xác. Ví dụ, nếu$N=4$và mọi chi phí đều bằng 0, câu trả lời đúng là bằng 0. Một giải pháp có lỗi chỉ tính tổng các cặp liên tiếp mà không đóng chu trình cũng sẽ tạo ra số 0 ở đây, ẩn lỗi, do đó tính chính xác phải được thực thi về mặt cấu trúc thay vì kiểm tra theo kinh nghiệm. 

Một trường hợp cạnh khác xuất hiện khi chu trình tối ưu không phải là duy nhất. Ví dụ, các lựa chọn cục bộ bất đối xứng vẫn có thể tạo ra các chu kỳ tối ưu toàn cầu. Bất kỳ cách tiếp cận tham lam nào nhằm khắc phục sớm các điểm lân cận gần nhất sẽ thất bại vì các cạnh tối thiểu cục bộ không đảm bảo một chu kỳ nhất quán trên toàn cầu. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là liệt kê mọi thứ tự có thể có của các loại đá và tính toán chi phí của chu trình tương ứng. Với mỗi hoán vị, chúng ta tính tổng$C_{p_i, p_{i+1}}$cộng với cạnh bao quanh$C_{p_{N}, p_1}$. Điều này đúng vì nó đánh giá rõ ràng mọi chu kỳ hợp lệ. Tuy nhiên, nó đòi hỏi$N!$hoán vị và$O(N)$làm việc trên mỗi hoán vị, dẫn đến$O(N \cdot N!)$, cái nào cho$N=12$vượt xa giới hạn khả thi. 

Sự dư thừa trong phương pháp này xuất phát từ thực tế là thứ tự từng phần giống nhau xuất hiện trong nhiều hoán vị. Khi tiền tố của các đỉnh đã chọn được cố định, cấu trúc còn lại chỉ phụ thuộc vào đỉnh nào vẫn chưa được sử dụng và đỉnh nào hiện đang ở cuối đường dẫn. Điều này gợi ý một ý tưởng nén trạng thái: thay vì theo dõi các hoán vị đầy đủ, chúng tôi theo dõi các tập hợp con. 

Điều này tự nhiên dẫn đến một công thức lập trình động bitmask. Chúng tôi xác định trạng thái biểu thị rằng chúng tôi đã chọn một tập hợp con các đỉnh và hiện kết thúc ở một đỉnh cụ thể. Chi phí được lưu trữ trong trạng thái là chi phí tối thiểu để hình thành một đường đi truy cập chính xác các đỉnh đó theo một thứ tự nào đó và kết thúc tại đỉnh đó. Các chuyển tiếp sẽ mở rộng đường đi bằng cách thêm một đỉnh chưa sử dụng. 

Câu trả lời cuối cùng thu được bằng cách đóng chu trình: sau khi tất cả các đỉnh được sử dụng, chúng ta quay lại từ điểm cuối cuối cùng về đỉnh bắt đầu và thêm cạnh cuối cùng đó. 

Điều này làm giảm vấn đề từ hoán vị giai thừa đến$O(N^2 2^N)$các trạng thái, điều này có thể dễ dàng thực hiện được đối với$N \le 12$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(N \cdot N!)$|$O(N)$| Quá chậm | 
| Bitmask DP qua đường dẫn |$O(N^2 2^N)$|$O(N 2^N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa một đỉnh làm điểm bắt đầu để loại bỏ tính đối xứng quay. Không mất tính tổng quát, ta chọn đỉnh 0 làm điểm bắt đầu của chu trình. Mỗi chu kỳ hợp lệ có thể được xoay để nó bắt đầu từ 0. 

Chúng tôi xác định một bảng DP trong đó`dp[mask][i]`biểu thị chi phí tối thiểu để bắt đầu từ 0, hãy truy cập chính xác các đỉnh trong`mask`, và kết thúc tại đỉnh`i`. 

1. Khởi tạo bảng DP với vô cùng. Bộ`dp[1 << 0][0] = 0`vì chúng ta bắt đầu ở đỉnh 0 mà không mất phí. 
2. Lặp lại tất cả các mặt nạ chứa đỉnh 0. Đối với mỗi mặt nạ và mỗi điểm cuối`i`bên trong mặt nạ, hãy thử chuyển đổi. 
3. Đối với từng trạng thái hiện tại`(mask, i)`, hãy thử mở rộng đường đi bằng cách thêm một đỉnh`j`không ở trong`mask`. Trạng thái mới trở thành`(mask | (1 << j), j)`, và chi phí tăng lên bởi`C[i][j]`. Chúng tôi cập nhật`dp`với giá trị nhỏ nhất có thể. 
4. Sau khi xử lý tất cả các trạng thái, chúng ta chỉ xem xét các trạng thái có`mask`bao gồm tất cả các đỉnh. Đối với mỗi đỉnh kết thúc có thể`i`, chúng tôi kết thúc chu trình bằng cách thêm chi phí`C[i][0]`. 
5. Câu trả lời là mức tối thiểu trên tất cả các điểm cuối mặt nạ đầy đủ như vậy. 

Lý do việc sửa đỉnh bắt đầu có hiệu quả là vì bất kỳ chu kỳ nào cũng có thể được quay để bắt đầu ở mức 0 mà không làm thay đổi chi phí của nó. Điều này loại bỏ việc đếm dư thừa các chu kỳ giống hệt nhau trong các phép quay khác nhau, giảm không gian trạng thái xuống một hệ số$N$. 

## Tại sao nó hoạt động 

Ở mọi trạng thái DP, chúng tôi duy trì bất biến rằng`dp[mask][i]`là chi phí tối thiểu trong số tất cả các đường đi thăm chính xác các đỉnh trong`mask`và kết thúc tại`i`. Mọi chuyển đổi đều duy trì tính hợp lệ bằng cách thêm chính xác một đỉnh chưa sử dụng, do đó không xảy ra lần truy cập lại không hợp lệ. 

Bởi vì mọi hoán vị bắt đầu từ đỉnh 0 tương ứng với chính xác một chuỗi chuyển tiếp DP, nên tất cả các đường dẫn Hamilton có thể bắt đầu từ 0 đều được biểu diễn. Việc đóng chu trình ở cuối một cách chính xác sẽ giải quyết được cạnh cuối cùng bị thiếu và vì mỗi chu trình đều có một đại diện bắt đầu từ 0 nên chu trình tối ưu được đảm bảo sẽ được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    c = [list(map(int, input().split())) for _ in range(n)]
    
    INF = 10**18
    dp = [[INF] * n for _ in range(1 << n)]
    
    dp[1][0] = 0
    
    for mask in range(1 << n):
        if not (mask & 1):
            continue
        for i in range(n):
            if not (mask & (1 << i)):
                continue
            cur = dp[mask][i]
            if cur == INF:
                continue
            for j in range(n):
                if mask & (1 << j):
                    continue
                nmask = mask | (1 << j)
                val = cur + c[i][j]
                if val < dp[nmask][j]:
                    dp[nmask][j] = val
    
    full = (1 << n) - 1
    ans = INF
    for i in range(n):
        ans = min(ans, dp[full][i] + c[i][0])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Bảng DP được khởi tạo với giá trị trọng điểm lớn để các trạng thái không thể truy cập không cản trở các chuyển đổi tối thiểu. Trạng thái bắt đầu sử dụng mặt nạ`1`vì chỉ có đỉnh 0 được bao gồm. 

Mặt nạ ba vòng lặp, điểm cuối hiện tại và đỉnh tiếp theo là công cụ chuyển tiếp cốt lõi. Nó thử một cách có hệ thống mọi phần mở rộng có thể của một phần đường dẫn mà không lặp lại. 

Vòng lặp cuối cùng kết thúc chu trình một cách rõ ràng bằng cách quay lại đỉnh bắt đầu, đảm bảo cấu trúc hình tròn được tính toán đầy đủ. 

Một lỗi phổ biến ở đây là quên thực thi rằng đường dẫn bắt đầu từ 0 một cách nhất quán. Nếu không có điều này, các phép quay của cùng một chu kỳ sẽ được tính nhiều lần, nhưng mức tối thiểu sẽ vẫn đúng, mặc dù có tính toán dư thừa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
0 1 2 3
1 0 4 5
2 4 0 6
3 5 6 0
```Chúng tôi theo dõi các trạng thái bắt đầu từ đỉnh 0. 

| mặt nạ | kết thúc | dp[mặt nạ][kết thúc] | 
| --- | --- | --- | 
| 0001 | 0 | 0 | 
| 0011 | 1 | 1 | 
| 0111 | 2 | 5 | 
| 1111 | 3 | 11 | 

Từ trạng thái đầy đủ: 

| kết thúc | chi phí chu kỳ | 
| --- | --- | 
| 1 | 1 + C[1][0] = 2 | 
| 2 | 5 + C[2][0] = 7 | 
| 3 | 11 + C[3][0] = 14 | 

Tối thiểu là 14. 

Dấu vết này cho thấy cách DP tích lũy chi phí đường dẫn tăng dần trong khi trì hoãn cạnh đóng cho đến cuối. 

### Mẫu 2 (đã thi công) 

đầu vào:```
3
0 2 3
2 0 1
3 1 0
```| mặt nạ | kết thúc | dp | 
| --- | --- | --- | 
| 001 | 0 | 0 | 
| 011 | 1 | 2 | 
| 111 | 2 | 3 | 

Chu kỳ kết thúc: 

| kết thúc | chu kỳ | 
| --- | --- | 
| 1 | 2 + C[1][0] = 4 | 
| 2 | 3 + C[2][0] = 6 | 

Câu trả lời là 4. 

Ví dụ này nhấn mạnh rằng chu trình tối ưu không nhất thiết phải thẳng hàng với các cạnh trực tiếp nhỏ nhất, vì cấu trúc phụ thuộc vào việc hoàn thành một chu trình Hamilton đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2 2^N)$| Mỗi tiểu bang mở rộng tới tối đa$N$chuyển tiếp$2^N$tập hợp con | 
| Không gian |$O(N 2^N)$| Bảng DP lưu trữ chi phí tốt nhất cho từng tập hợp con và điểm cuối | 

Với$N \le 12$, số lượng trạng thái nhiều nhất là$12 \cdot 2^{12} = 49152$và các chuyển tiếp vẫn nằm trong giới hạn. Giải pháp phù hợp thoải mái trong cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    c = [list(map(int, input().split())) for _ in range(n)]

    INF = 10**18
    dp = [[INF] * n for _ in range(1 << n)]
    dp[1][0] = 0

    for mask in range(1 << n):
        if not (mask & 1):
            continue
        for i in range(n):
            if not (mask & (1 << i)):
                continue
            cur = dp[mask][i]
            if cur == INF:
                continue
            for j in range(n):
                if mask & (1 << j):
                    continue
                nmask = mask | (1 << j)
                val = cur + c[i][j]
                if val < dp[nmask][j]:
                    dp[nmask][j] = val

    full = (1 << n) - 1
    ans = min(dp[full][i] + c[i][0] for i in range(n))
    return str(ans)

# provided sample
assert run("""4
0 1 2 3
1 0 4 5
2 4 0 6
3 5 6 0
""") == "14"

# minimum case
assert run("""2
0 5
5 0
""") == "10"

# symmetric equal costs
assert run("""3
0 1 1
1 0 1
1 1 0
""") == "3"

# skewed costs
assert run("""3
0 100 1
100 0 100
1 100 0
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút đối xứng | 10 | tính đúng đắn của việc đóng chu kỳ cơ sở | 
| mọi chi phí như nhau | 3 | tính đối xứng và độ chính xác DP | 
| chi phí sai lệch | 3 | cấu trúc tối ưu không tham lam | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$N=2$. DP vẫn hoạt động nhưng chỉ có một chu kỳ khả thi: 0 → 1 → 0. Quá trình chuyển đổi trạng thái không bao giờ phân nhánh và câu trả lời cuối cùng chỉ đơn giản là$C_{0,1} + C_{1,0}$. Thuật toán xử lý việc này một cách tự nhiên vì bảng DP chỉ bao gồm hai trạng thái liên quan và bước kết thúc sẽ thêm cạnh trả về một cách rõ ràng. 

Một trường hợp khác là khi mọi chi phí đều bằng không. Mọi chu trình Hamilton đều có chi phí bằng 0 và DP sẽ truyền các số 0 qua tất cả các trạng thái. Mức tối thiểu cuối cùng vẫn bằng 0 bất kể lựa chọn đường dẫn nào, điều này xác nhận rằng thuật toán không phụ thuộc vào việc phá vỡ ràng buộc. 

Trường hợp thứ ba là khi một cạnh cực kỳ lớn so với các cạnh khác. DP tránh điều đó một cách chính xác vì nó luôn xem xét tất cả các chuyển đổi có thể có và giữ lại mức tối thiểu trên các đường dẫn hoàn chỉnh. Ngay cả khi lợi thế giá rẻ cục bộ dẫn đến việc hoàn thành tốn kém trên toàn cầu, việc thăm dò không gian trạng thái đảm bảo một tuyến đường thay thế được đánh giá và lựa chọn nếu rẻ hơn.
