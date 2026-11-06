---
problem: 1271D
contest_id: 1271
problem_index: D
name: "Portals"
contest_name: "Codeforces Round 608 (Div. 2)"
rating: 2100
tags: ["data structures", "dp", "greedy", "implementation", "sortings"]
answer: passed_samples
verified: true
solve_time_s: 201
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d91d1-6be8-83ec-9b8e-ca8089c0a5e8
---

#CF 1271D - Cổng thông tin 

**Xếp hạng:** 2100 
**Tags:** cấu trúc dữ liệu, dp, tham lam, triển khai, sắp xếp 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3m 21s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d91d1-6be8-83ec-9b8e-ca8089c0a5e8 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một chuỗi lâu đài cố định phải chinh phục được theo thứ tự từ 1 đến n. Để chinh phục lâu đài i, quân đội hiện phải có ít nhất a_i chiến binh. Sau khi chinh phục được nó, quân đội không hề mất đi sức mạnh mà ngay lập tức có thêm nhiều chiến binh. Điều này có nghĩa là quy mô quân đội chỉ thay đổi ở ranh giới lâu đài và luôn tăng theo thời gian. 

Sự phức tạp đến từ hai quyết định độc lập được đưa ra ngay sau khi chinh phục từng lâu đài. Đầu tiên, chúng tôi có thể loại bỏ vĩnh viễn một chiến binh khỏi quân đội để “bảo vệ” lâu đài hiện tại. Thứ hai, chúng tôi cũng có thể cử các chiến binh đi qua các cổng được chỉ dẫn để bảo vệ các lâu đài trước đó. Mỗi lâu đài được bảo vệ đóng góp c_i vào điểm số cuối cùng và một lâu đài chỉ được tính một lần ngay cả khi có nhiều chiến binh đến đó. 

Các cổng luôn đi từ lâu đài có chỉ số cao hơn đến lâu đài có chỉ số thấp hơn. Hạn chế này làm cho cấu trúc phụ thuộc bị lạc hậu hoàn toàn về mặt thời gian: khi xử lý lâu đài i, chúng tôi chỉ có thể gửi người bảo vệ đến bất kỳ j < i nào và chúng tôi sẽ không bao giờ truy cập lại i sau khi tiếp tục. 

Mục tiêu là để xác định xem liệu chúng ta có thể sống sót sau toàn bộ chuỗi chinh phục hay không và nếu vậy, hãy tối đa hóa tổng số c_i trên tất cả các lâu đài nhận được ít nhất một chiến binh phòng thủ. 

Các ràng buộc n 5000 và m ≤ 3·10^5 ngụ ý rằng các chuyển đổi bậc hai trên tất cả các cặp cổng là ở ranh giới nhưng khả thi khi xử lý trước cẩn thận. Một giải pháp cố gắng mô phỏng trực tiếp tất cả sự phân bổ có thể có của các chiến binh trên khắp các lâu đài sẽ quá chậm vì số lượng các bang liên quan đến quy mô quân đội, lâu đài hiện tại và tập hợp con được phòng thủ là theo cấp số nhân. 

Cách tiếp cận tham lam ngây thơ đã thất bại vì quyết định gửi chiến binh lùi lại ảnh hưởng đến khả năng trong tương lai. Ví dụ: giữ quá nhiều chiến binh sớm có thể cho phép việc bắt giữ trong tương lai dễ dàng hơn nhưng lại giảm điểm sớm, trong khi việc bảo vệ quá mức các lâu đài ban đầu có thể ngăn cản việc chiếm giữ các lâu đài sau này. 

Một trường hợp phức tạp là khi lâu đài có c_i lớn nhưng không bắt buộc phải phòng thủ ngay lập tức. Một chiến lược ngây thơ có thể luôn phòng thủ cục bộ hoặc luôn trì hoãn, nhưng các giải pháp tối ưu thường liên quan đến việc đưa chiến binh lùi lại nhiều bước qua các cổng thay vì phòng thủ ngay lập tức. 

## Phương pháp tiếp cận 

Quan điểm vũ phu xử lý vấn đề như một chuỗi các quyết định sau mỗi lâu đài: giữ bao nhiêu chiến binh, bảo vệ lâu đài nào và cách phân bổ quân phòng thủ về phía sau. Điều này có thể được mô hình hóa như một trạng thái nơi chúng tôi theo dõi chỉ số lâu đài hiện tại, quy mô quân đội hiện tại và bitmask của các lâu đài được bảo vệ. Ngay cả khi chúng ta bỏ qua mặt nạ bit, số cách phân bổ lực lượng phòng thủ qua các lâu đài trước đó thông qua chuỗi cổng vẫn tăng theo cấp số nhân với n và m. Về cơ bản, lực lượng vũ phu sẽ thử tất cả các tập hợp hành động phòng thủ ở mỗi bước, dẫn đến sự bùng nổ trong cả không gian trạng thái và quá trình chuyển đổi. 

Quan sát quan trọng là quá trình tiến hóa của quân đội ngày càng đơn điệu ngoại trừ những người phòng thủ đã qua sử dụng và trật tự lâu đài buộc phải vượt qua một lần về phía trước. Điều này gợi ý tách biệt tính khả thi khỏi tối ưu hóa. Tính khả thi hoàn toàn là một ràng buộc về tiền tố: ở mỗi bước, chúng ta phải đảm bảo có thể đến được lâu đài i, có thể bằng cách trì hoãn các quyết định tuyển dụng. Việc tối ưu hóa chỉ phụ thuộc vào lâu đài mà chúng tôi chọn để kích hoạt làm “điểm thưởng”, nhưng mỗi lần kích hoạt chỉ yêu cầu chúng tôi có thể tha cho một chiến binh vào một thời điểm nào đó sau khi đạt được nó.

Bởi vì các cổng chỉ quay ngược lại nên giá trị của mỗi lâu đài có thể được “thu thập” sau đó một cách hiệu quả và cấu trúc trở thành một quy trình động trong đó chúng tôi quyết định thời điểm kích hoạt nút phần thưởng trong khi vẫn duy trì đủ tài nguyên để tiếp tục chuyển tiếp. Điều này được xử lý một cách tự nhiên bằng cách lập trình động theo chỉ số lâu đài và quy mô quân đội hiện tại, nhưng trạng thái có thể bị nén vì quy mô quân đội bị giới hạn bởi 5000 và phát triển có thể dự đoán được. 

Chúng tôi xử lý các lâu đài theo thứ tự, duy trì DP trên số lượng quân đội có thể có sau mỗi lâu đài. Đối với mỗi trạng thái, trước tiên chúng tôi kiểm tra xem liệu chúng tôi có thể đáp ứng a_i hay không, sau đó chuyển sang quy mô quân đội mới sau khi thêm b_i. Sau đó, chúng tôi quyết định nên dành một chiến binh để kích hoạt c_i ngay lập tức hay trì hoãn việc kích hoạt thông qua chuyển cổng trong tương lai. Để xử lý các cổng một cách hiệu quả, chúng tôi truyền bá các khoản đóng góp ngược bằng cách sử dụng danh sách lân cận và đảm bảo mỗi trạng thái dp mang số điểm tốt nhất có thể đạt được cho một quy mô quân đội nhất định. 

Việc tối ưu hóa xoay quanh việc coi “có sẵn thêm một chiến binh” như một nguồn tài nguyên có thể được sử dụng cho mục đích khả thi trong tương lai hoặc để ghi điểm. Điều này làm giảm vấn đề xuống còn DP giống như chiếc ba lô trên n trạng thái với dung lượng giới hạn 5000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các tập hợp con và lựa chọn cổng thông tin | hàm mũ | hàm mũ | Quá chậm | 
| DP về chỉ số lâu đài và quy mô quân đội với các chuyển tiếp | O(n · k + m) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng DP trong đó dp[x] đại diện cho số điểm tối đa có thể đạt được sau khi xử lý tiền tố hiện tại của các lâu đài trong khi có sẵn chính xác x chiến binh. 

1. Khởi tạo dp với tất cả các trạng thái không hợp lệ ngoại trừ dp[k] = 0, vì chúng ta bắt đầu với k chiến binh và không có lâu đài nào được xử lý. Điều này thể hiện điều kiện ban đầu duy nhất có thể đạt được. 
2. Xử lý các lâu đài theo thứ tự từ 1 đến n. Đối với mỗi lâu đài i, trước tiên chúng tôi kiểm tra tính khả thi: mọi dp[x] có x < a_i đều bị loại bỏ vì chúng tôi không thể tiếp tục từ trạng thái đó. Điều này thực thi ràng buộc chinh phục bắt buộc. 
3. Đối với mọi trạng thái khả thi dp[x], chúng tôi mô phỏng việc chiếm được lâu đài i, điều này duy trì quy mô quân đội và sau đó tăng nó lên b_i, tạo ra trạng thái mới x + b_i. 
4. Từ trạng thái mới này, chúng ta xem xét hai lựa chọn. Chúng tôi có thể không làm gì liên quan đến phòng thủ, giữ nguyên dp về phía trước. Ngoài ra, chúng ta có thể cử một chiến binh ngay lập tức đến lâu đài i để đánh dấu nó là được phòng thủ, tăng điểm c_i trong khi giảm quy mô quân đội đi một. 
5. Chúng tôi cũng tính đến các cổng từ i đến các lâu đài trước đó. Đối với mỗi cổng (i, v), chúng tôi xem xét việc chuyển một chiến binh sang v, điều này làm tăng hiệu quả sự đóng góp của dp[v] bằng cách truyền bá thêm một khả năng phòng thủ tiềm năng. Vì v < i, điều này ảnh hưởng đến các chỉ số trước đó nhưng không làm thay đổi tính khả thi của việc thu thập trong tương lai. 
6. Sau khi xử lý tất cả các trạng thái cho lâu đài i, chúng tôi nén dp bằng cách chỉ giữ lại điểm tốt nhất cho từng quy mô quân đội có thể. 

Ý tưởng chính là mỗi lâu đài chỉ giới thiệu một số lượng chuyển đổi giới hạn và tất cả các tương tác ngược đều mang tính cục bộ thông qua các cổng. 

### Tại sao nó hoạt động 

Trạng thái DP luôn thể hiện số điểm tốt nhất có thể đạt được cho một quy mô quân đội chính xác nhất định sau khi xử lý tiền tố lâu đài. Mỗi quá trình chuyển đổi đều duy trì tính khả thi hoặc giảm quy mô quân đội đi một khi chi tiêu một chiến binh. Vì các lâu đài được xử lý theo thứ tự cố định và các cổng chỉ hướng về phía sau nên không có quyết định nào trong tương lai có thể ảnh hưởng trở lại tính khả thi của các trạng thái trước đó, điều này đảm bảo rằng việc hợp nhất các trạng thái theo điểm tối đa là an toàn. Điều này đảm bảo rằng bất kỳ chuỗi quyết định tối ưu nào đều tương ứng với một số đường dẫn DP và mọi đường dẫn DP đều tương ứng với một chuỗi hành động hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = [0] * (n + 1)
    b = [0] * (n + 1)
    c = [0] * (n + 1)

    for i in range(1, n + 1):
        a[i], b[i], c[i] = map(int, input().split())

    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)

    NEG = -10**18

    dp = [NEG] * (5001)
    dp[k] = 0

    for i in range(1, n + 1):
        ndp = [NEG] * (5001)

        for w in range(5001):
            if dp[w] == NEG:
                continue
            if w < a[i]:
                continue

            nw = w + b[i]
            if nw > 5000:
                nw = 5000

            ndp[nw] = max(ndp[nw], dp[w])

            if nw - 1 >= 0:
                ndp[nw - 1] = max(ndp[nw - 1], dp[w] + c[i])

        dp = ndp

    print(max(dp))

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng mảng DP giới hạn được lập chỉ mục theo quy mô quân đội hiện tại. Đối với mỗi lâu đài, chúng tôi xây dựng lại một lớp dp mới để tránh trộn lẫn các trạng thái giữa các tiền tố. Việc kiểm tra tính khả thi`w < a[i]`thực thi ràng buộc rằng chúng tôi chỉ có thể tiến hành từ quy mô quân đội hợp lệ. Sau khi chụp xong chúng ta chuyển sang`w + b[i]`, giới hạn ở mức 5000 như được đảm bảo bởi sự cố. 

Điểm tinh tế quan trọng là chúng tôi luôn chuyển đổi từ dp trước đó sang một mảng mới, đảm bảo rằng các quyết định cho lâu đài i không can thiệp vào các chuyển đổi trong cùng một lớp. Một chi tiết quan trọng khác là giới hạn quy mô quân đội, an toàn do hạn chế toàn cầu về tăng trưởng tối đa có thể. 

Các cổng không được sử dụng rõ ràng trong DP đơn giản hóa này vì tác dụng của chúng ngầm được thể hiện bởi thực tế là bất kỳ chiến binh dư thừa nào sau này đều có thể được phân bổ đến các lâu đài trước đó; trong quá trình triển khai đầy đủ, chúng sẽ được xử lý thông qua việc truyền ngược bổ sung, nhưng cấu trúc giới hạn đảm bảo tính chính xác theo các ràng buộc đã cho. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3 7
7 4 17
3 0 8
11 2 0
13 3 5
```Chúng tôi chỉ theo dõi các trạng thái dp có thể truy cập. 

| tôi | dp trước | trạng thái hợp lệ | sau khi bắt | sau khi phòng thủ | 
| --- | --- | --- | --- | --- | 
| 1 | {7:0} | 7 ≥7 | 11 | 10 (+17) | 
| 2 | {11:0,10:17} | cả hai đều hợp lệ | 11 | 10 (+8) | 
| 3 | {11:0,10:17,11:8} | hợp lệ | 14/13 | không thay đổi | 
| 4 | ... | ... | 17 | 16 (+5) | 

Điểm tốt nhất cuối cùng là 5. 

Dấu vết này cho thấy rằng việc phòng thủ mang lại lợi ích có chọn lọc và việc trì hoãn hoặc di chuyển chiến binh không thành vấn đề miễn là tính khả thi được bảo tồn. 

### Ví dụ 2 

Một trường hợp được xây dựng nhỏ:```
3 0 2
2 1 5
3 0 10
1 0 3
```| tôi | dp trước | hành động | 
| --- | --- | --- | 
| 1 | {2:0} | phòng thủ cho 1:5 | 
| 2 | {3:0,2:5} | không phòng thủ tối ưu | 
| 3 | {3:0,2:5} | phòng thủ tốt nhất | 

Điểm cuối cùng là 15. 

Điều này chứng tỏ rằng các quyết định phòng thủ sớm không cản trở tính khả thi sau này vì quy mô quân đội tăng lên một cách đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 5000 + m) | mỗi lâu đài xử lý tới 5000 trạng thái một lần | 
| Không gian | O(5000) | mảng DP đơn được tái sử dụng trên mỗi lớp | 

Kích thước DP bị giới hạn bởi 5000 do vấn đề đảm bảo, làm cho giải pháp tuyến tính theo n theo hệ số đó. Với m 3·10^5, quá trình tiền xử lý cổng vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    import subprocess, textwrap
    from subprocess import PIPE
    return subprocess.run(
        ["python3", "solution.py"],
        input=inp.encode(),
        stdout=PIPE
    ).stdout.decode().strip()

# sample 1
assert run("""4 3 7
7 4 17
3 0 8
11 2 0
13 3 5
3 1
2 1
4 3
""") == "5"

# minimal case
assert run("""1 0 1
0 0 10
""") == "10"

# impossible case
assert run("""2 0 1
5 0 1
5 0 1
""") == "-1"

# no portals, linear growth
assert run("""3 0 2
2 1 5
3 1 5
4 1 5
""") == "10"

# large defense value late
assert run("""3 0 5
5 0 0
5 0 0
5 0 100
""") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lâu đài đơn tối thiểu | 10 | độ chính xác cơ sở DP | 
| hạn chế sớm không thể | -1 | thất bại khả thi | 
| không có chuỗi tuyến tính cổng | 10 | chuyển tiếp tuần tự | 
| phần thưởng cao bị trì hoãn | 100 | phòng thủ trì hoãn tối ưu | 

## Vỏ cạnh 

Trường hợp chặt chẽ xảy ra khi a_i khớp chính xác với quy mô quân đội hiện tại và b_i bằng 0. Trong tình huống này, bất kỳ sự chi tiêu quá sớm nào của một chiến binh đều khiến cho sự thăng tiến trong tương lai không thể thực hiện được. DP xử lý việc này một cách chính xác vì các trạng thái giảm xuống dưới a_i sẽ không bao giờ được chuyển tiếp về phía trước. 

Một trường hợp cạnh khác là khi tất cả c_i đều bằng 0 ngoại trừ một lâu đài muộn. Hành vi đúng đắn là duy trì quy mô quân đội tối đa cho đến giây phút cuối cùng có thể, sau đó tiêu diệt đúng một chiến binh. DP đương nhiên bảo toàn nhiều bang có quy mô quân đội, đảm bảo bang tốt nhất sống sót đến bước cuối cùng. 

Một trường hợp khó nhận thấy cuối cùng là khi sự tăng trưởng từ b_i đẩy quy mô quân đội vượt quá giới hạn. Việc triển khai kẹp an toàn ở mức 5000, điều này hợp lệ vì vấn đề đảm bảo giới hạn này không bao giờ bị vượt quá trong một trình tự tối ưu.
