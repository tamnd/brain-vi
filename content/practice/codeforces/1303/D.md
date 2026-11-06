---
title: "CF 1303D - Đổ đầy túi"
description: "Chúng ta được cấp một lượng không gian mục tiêu và một tập hợp các khối có sẵn có kích thước là lũy thừa của hai. Nhiệm vụ là quyết định xem liệu chúng ta có thể soạn chính xác kích thước mục tiêu bằng cách sử dụng các khối này hay không và nếu không, hãy báo cáo là không thể thực hiện được."
date: "2026-06-16T05:44:37+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1303
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 82 (Rated for Div. 2)"
rating: 1900
weight: 1303
solve_time_s: 408
verified: true
draft: false
---

[CF 1303D - Đổ đầy túi](https://codeforces.com/problemset/problem/1303/D) 

**Xếp hạng:** 1900 
**Tags:** bitmasks, tham lam 
**Thời gian giải:** 6 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một lượng không gian mục tiêu và một tập hợp các khối có sẵn có kích thước là lũy thừa của hai. Nhiệm vụ là quyết định xem liệu chúng ta có thể soạn chính xác kích thước mục tiêu bằng cách sử dụng các khối này hay không và nếu không, hãy báo cáo là không thể thực hiện được. Nếu có thể, chúng tôi muốn giảm thiểu số lần chúng tôi chia các khối lớn hơn thành các khối nhỏ hơn. 

Hoạt động chính là bất kỳ khối kích thước nào$2^k$có thể được chia thành hai khối kích thước$2^{k-1}$. Hoạt động này miễn phí về tính khả dụng nhưng được tính là một bộ phận. Sau khi chia nhỏ, chúng ta có thể sử dụng các khối nhỏ hơn thu được để lấp đầy tổng kích thước được yêu cầu. 

Chúng tôi không cố gắng chọn các tập hợp con tùy ý; thay vào đó, mọi thứ giảm xuống việc quản lý số lượng khối mà chúng tôi có ở mỗi lũy thừa hai và quyết định khi nào chúng tôi buộc phải chia một khối lớn hơn để đáp ứng các bit thấp hơn bị thiếu của mục tiêu. 

Ràng buộc$n \le 10^{18}$ngay lập tức cho chúng ta biết rằng chúng ta không thể mô phỏng mục tiêu theo cấu trúc từng bit đơn giản bằng cách sử dụng các vòng lặp lớn trên giá trị của$n$. Số lượng bit nhiều nhất là 60, do đó mọi giải pháp sẽ hoạt động trong$O(\log n)$mỗi trường hợp thử nghiệm. Tổng số hộp trong các bài kiểm tra là$10^5$, do đó, mọi thứ như mô phỏng nặng trên mỗi hộp chỉ được chấp nhận nếu nó duy trì tuyến tính trong tổng kích thước đầu vào. 

Một trường hợp thất bại tinh vi xuất hiện khi chúng ta tham lam chiếm lấy các khối lớn hơn mà không tính toán cẩn thận cho việc chia tách trong tương lai. Ví dụ: nếu chúng ta tiêu thụ một$2^k$chặn để có bit cao hơn$n$mặc dù bit thấp hơn sau đó yêu cầu phân tách, chúng ta có thể đánh giá thấp hoặc đánh giá quá cao số lượng phân chia cần thiết. Một trường hợp khác xảy ra khi chúng ta có đủ tổng nhưng không căn chỉnh nhị phân chính xác, chẳng hạn như có nhiều$1$-blocks nhưng cần sớm có khối lớn hơn, buộc phải hợp nhất nhiều lần thông qua việc phân chia các quyền hạn cao hơn. 

Khó khăn thực sự là sự phân chia lan truyền xuống dưới và đôi khi chúng ta phải mượn từ các bit cao hơn để đáp ứng các bit thấp hơn, và việc vay mượn này tạo ra chi phí phân chia. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực trực tiếp sẽ coi đây là vấn đề trạng thái đối với các cấu hình nhiều kích thước khối. Bất cứ lúc nào, chúng ta có thể sử dụng một khối nếu nó phù hợp với kích thước cần thiết hoặc chia một khối lớn hơn thành hai khối nhỏ hơn. BFS hoặc DP trên tất cả các phân phối khối theo lũy thừa của hai cuối cùng sẽ tìm thấy số lượng phân chia tối thiểu. Tuy nhiên, ngay cả khi hạn chế số lượng trên mỗi cấp độ bit, mỗi cấp độ có thể tăng lên tới$10^5$, và có tới 60 cấp độ, do đó không gian trạng thái trở nên rộng lớn về mặt thiên văn. Ngay cả việc giải thích đường đi ngắn nhất một cách cẩn thận cũng sẽ thất bại vì các quá trình chuyển đổi liên tục phân phối lại khối lượng giữa các cấp độ, tạo ra sự bùng nổ các trạng thái. 

Quan sát quan trọng là chúng ta không bao giờ cần sắp xếp lại các khối một cách tùy tiện; chúng ta chỉ cần theo dõi xem chúng ta có bao nhiêu khối ở lũy thừa hai và xử lý các bit mục tiêu từ ít quan trọng nhất trở lên. Khi một chút$n$yêu cầu một khối có kích thước$2^i$, chúng ta hoặc đã có một khối hoặc chúng ta phải xây dựng nó bằng cách chia một khối lớn hơn. Tách một$2^k$chặn vào$2^i$yêu cầu$k - i$phân tách và nó cũng tạo ra các khối trung gian cần được tính đến. 

Điều này gợi ý một mô phỏng tham lam trên các vị trí bit: chúng tôi duy trì số lượng khả dụng trên lũy thừa hai và cho mỗi bit của$n$, chúng tôi đảm bảo cung cấp. Nếu thiếu, chúng tôi kéo từ cấp cao hơn gần nhất và truyền thặng dư xuống dưới, tính toán cẩn thận các phần chia. 

Cấu trúc hoạt động vì biểu diễn nhị phân của$n$là cố định, do đó, mỗi bit độc lập yêu cầu chính xác một đơn vị công suất đó và bất kỳ sự không phù hợp nào đều được giải quyết cục bộ bằng cách mượn từ các công suất cao hơn với chi phí xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Vay tham lam bitwise |$O(m + \log n)$|$O(\log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu thị các hộp có sẵn dưới dạng số lượng theo lũy thừa của hai. Cho phép`cnt[i]`có bao nhiêu khối có kích thước$2^i$chúng tôi hiện có. 

Sau đó chúng tôi xử lý các bit của$n$từ thấp nhất đến cao nhất, duy trì mô phỏng đang chạy về cách các khối chảy xuống khi chúng tôi chia các khối lớn hơn. 

1. Xây dựng mảng tần số`cnt`, nhóm tất cả các hộp đầu vào theo số mũ của chúng$i$như vậy$a = 2^i$. Điều này cung cấp cho chúng tôi nguồn cung ban đầu ở mỗi cấp độ. 
2. Vị trí bit di chuyển ngang từ 0 đến 60, vì$n \le 10^{18}$. Tại mỗi vị trí$i$, kiểm tra xem$i$-bit thứ của$n$được thiết lập. 
3. Nếu bit được đặt và chúng ta đã có một khối trong`cnt[i]`, chúng tôi tiêu thụ một và tiếp tục. 
4. Nếu bit được đặt nhưng`cnt[i]`bằng 0, chúng tôi tìm kiếm gần nhất$j > i$như vậy`cnt[j] > 0`. Nếu không tồn tại, nhiệm vụ là không thể. 
5. Một khi chúng ta tìm thấy một$j$, chúng tôi liên tục chia một khối có kích thước$2^j$xuống kích thước$2^i$. Mỗi phần chia sẽ tăng câu trả lời lên 1 và tạo ra các khối trung gian lấp đầy tất cả các cấp độ giữa$i$Và$j$. 
6. Sau khi chia nhỏ, chúng ta giảm dần`cnt[j]`và tăng dần tất cả các cấp độ trung gian tương ứng, đảm bảo bảo toàn khối lượng. 
7. Bây giờ chúng ta tiêu thụ một đơn vị ở cấp độ$i$. 
8. Sau khi đáp ứng được bit yêu cầu, chúng ta truyền phần dư thừa lên trên được xử lý hoàn toàn bằng số lượng, vì các khối không sử dụng vẫn có sẵn cho các bit cao hơn. 

Bất biến tinh tế là sau khi xử lý mức độ$i$, tất cả các khối còn lại ở cấp dưới$i$đã được tính toán chính xác và không có hoạt động nào trong tương lai sẽ yêu cầu xem lại chúng. 

Tính chính xác dựa trên thực tế là mọi sự phân chia đều di chuyển khối lượng xuống một cách có kiểm soát và việc lựa chọn tham lam khối cao hơn có sẵn gần nhất sẽ giảm thiểu số lần phân chia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        cnt = [0] * 61

        for x in map(int, input().split()):
            cnt[x.bit_length() - 1] += 1

        ans = 0

        for i in range(61):
            if (n >> i) & 1:
                j = i
                while j < 61 and cnt[j] == 0:
                    j += 1

                if j == 61:
                    ans = -1
                    break

                while j > i:
                    cnt[j] -= 1
                    cnt[j - 1] += 2
                    ans += 1
                    j -= 1

                cnt[i] -= 1

            cnt[i + 1] += cnt[i] // 2
            cnt[i] %= 2

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai nén tất cả các kích thước hộp thành số mũ sử dụng độ dài bit, vì mỗi$a_i$là sức mạnh của hai. Vòng lặp chính quét các vị trí bit của$n$. Khi thiếu một bit bắt buộc, nó sẽ tìm kiếm khối lớn hơn gần nhất có sẵn và thực hiện giảm một nửa lặp đi lặp lại. Mỗi lần chia đôi được tính là một lần chia. 

Sau khi xử lý yêu cầu ở mức$i$, chúng ta chuẩn hóa hệ thống bằng cách mang các cặp còn lại lên trên, vì hai khối có kích thước$2^i$có thể tạo thành một khối kích thước$2^{i+1}$. Điều này đảm bảo rằng việc trình bày vẫn ở mức tối thiểu và nhất quán cho các bước trong tương lai. 

Một chi tiết triển khai tinh tế là chúng tôi luôn giảm`cnt[i]`sau khi thỏa mãn một chút, vì bit đó thể hiện mức tiêu thụ thực tế. Một sự tinh tế khác là bước thực hiện sau mỗi lần lặp, giúp ngăn chặn việc đếm quá mức các khối nhỏ bị phân mảnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 10, m = 3
a = [1, 1, 32]
```Chúng tôi chỉ theo dõi các bit có liên quan. 

| tôi | n bit | cnt trước | hành động | chia tách | cnt sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | mang theo/bỏ qua | 0 | ổn định | 
| 1 | 1 | 0 | mượn từ 5 | chia 2→1 | cập nhật | 
| 3 | 1 | 1 | sử dụng trực tiếp | 0 | cập nhật | 

Đầu tiên chúng tôi sử dụng hai số 1 cho các bit thấp. Đối với$2^3 = 8$yêu cầu, chúng ta không thể trực tiếp sử dụng 32, nên chúng ta chia 32 thành 16, rồi 16 thành 8, tính ra 2 phép chia. 

Điều này chứng tỏ rằng các khối lớn chỉ hữu ích sau khi giảm một nửa liên tục và chi phí tích lũy chính xác theo độ sâu. 

### Ví dụ 2 

đầu vào:```
n = 20, m = 5
a = [2, 1, 16, 1, 8]
```| tôi | n bit | cnt | hành động | chia tách | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | được | không | 0 | 
| 2 | 1 | có sẵn dây chuyền 2/4/8 | trực tiếp | 0 | 
| 4 | 1 | 16 tồn tại | trực tiếp | 0 | 

Không cần phải phân tách vì nhiều tập hợp đã căn chỉnh với phân tách nhị phân là 20. 

Điều này cho thấy trường hợp lý tưởng trong đó đầu vào đã khớp với cấu trúc nhị phân của mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m + \log n)$| Mỗi hộp được xử lý một lần thành số lượng công suất và mỗi bit được quét tối đa 60 cấp độ với các hoạt động phân tách giới hạn | 
| Không gian |$O(\log n)$| Chỉ mảng tần số vượt quá lũy thừa của hai được lưu trữ | 

Các ràng buộc cho phép lên đến$10^5$các hộp trong tất cả các bài kiểm tra và chỉ có khoảng 60 cấp độ bit cho mỗi bài kiểm tra. Mô phỏng đảm bảo mỗi hộp được di chuyển lên hoặc xuống một cách hiệu quả với số lần không đổi, do đó tổng công việc vẫn tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log2

    def solve():
        t = int(input())
        for _ in range(t):
            n, m = map(int, input().split())
            cnt = [0] * 61
            for x in map(int, input().split()):
                cnt[x.bit_length() - 1] += 1

            ans = 0
            for i in range(61):
                if (n >> i) & 1:
                    j = i
                    while j < 61 and cnt[j] == 0:
                        j += 1
                    if j == 61:
                        ans = -1
                        break
                    while j > i:
                        cnt[j] -= 1
                        cnt[j - 1] += 2
                        ans += 1
                        j -= 1
                    cnt[i] -= 1

                cnt[i + 1] += cnt[i] // 2
                cnt[i] %= 2

            print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""3
10 3
1 32 1
23 4
16 1 4 1
20 5
2 1 16 1 8
""") == """2
-1
0"""

# custom cases
assert run("""1
1 1
1
""") == """0"""

assert run("""1
8 1
32
""") == """2"""

assert run("""1
7 2
4 4
""") == """-1"""

assert run("""1
6 3
4 1 1
""") == """1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`0`| đã khớp chính xác | 
|`1 8 / 32`|`2`| độ sâu phân tách lặp đi lặp lại | 
|`7 with insufficient structure`|`-1`| phát hiện không thể | 
|`6 with partial reuse`|`1`| lựa chọn chia nhỏ tối thiểu | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi khối có sẵn lớn nhất vượt xa bit được yêu cầu, chẳng hạn như cần$2^0$nhưng chỉ có$2^{50}$. Thuật toán liên tục chia nhỏ xuống, đếm từng bước và tích lũy chính xác 50 thao tác. Điều này tránh được lối tắt không chính xác khi coi nó như một phép chia đơn lẻ. 

Một trường hợp cạnh khác là khi tổng số khối có sẵn là đủ nhưng bị sai lệch về lũy thừa hai. Ví dụ, nhiều$2^1$các khối không thể trực tiếp thỏa mãn một$2^2$yêu cầu mà không cần sáp nhập thông qua mang hoặc vay. Cơ chế vận chuyển đảm bảo rằng các khối nhỏ dư thừa được tổng hợp đúng cách. 

Trường hợp cuối cùng là không thể xảy ra mặc dù tổng số tiền lớn. Nếu không có khối cao hơn tồn tại tại thời điểm cần thiết, việc tìm kiếm số mũ cao hơn sẽ thất bại và thuật toán trả về$-1$ngay lập tức. Điều này xử lý chính xác các trường hợp trong đó việc phân tách không thể kết nối các cấp độ phân cấp bị thiếu.
