---
problem: 1334B
contest_id: 1334
problem_index: B
name: "Middle Class"
contest_name: "Educational Codeforces Round 85 (Rated for Div. 2)"
rating: 1100
tags: ["greedy", "sortings"]
answer: passed_samples
verified: true
solve_time_s: 150
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e0b1f-105c-83ec-b139-a797acfe317f
---

#CF 1334B - Tầng lớp trung lưu 

**Đánh giá:** 1100 
**Tags:** tham lam, phân loại 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e0b1f-105c-83ec-b139-a797acfe317f 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được giao cho một nhóm người, mỗi người có một số tiền. Người “giàu có” được định nghĩa là người có số tiền cuối cùng ít nhất phải đạt ngưỡng$x$. 

Chúng ta được phép thực hiện một thao tác bao nhiêu lần cũng được. Mỗi hoạt động chọn bất kỳ tập hợp con người nào, lấy tất cả tiền từ tập hợp con đó và sau đó phân phối lại số tiền đó một cách đồng đều cho những người được chọn giống nhau. Sau khi phân phối lại, tổng số tiền bên trong tập hợp con vẫn giữ nguyên, nhưng tất cả những người được chọn đều có giá trị giống hệt nhau. 

Nhiệm vụ là xác định số lượng người tối đa có thể đạt được ít nhất$x$sau khi thực hiện bất kỳ chuỗi hoạt động nào như vậy. 

Các ràng buộc ngụ ý đến$10^5$mọi người trên tất cả các trường hợp thử nghiệm, vì vậy chúng tôi cần$O(n \log n)$hoặc$O(n)$giải pháp cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào mô phỏng các phép toán hoặc xem xét trực tiếp các tập hợp con đều không khả thi vì số lượng tập hợp con là theo cấp số nhân. 

Một điểm tinh tế quan trọng là tiền có thể được chuyển nhượng hoàn hảo trong các nhóm đã chọn, bao gồm cả các giá trị phân số. Điều này có nghĩa là chúng ta được phép phân phối lại tổng tài sản một cách hiệu quả một cách tùy ý cho các nhóm đã chọn, nhưng chỉ thông qua các hoạt động cân bằng. 

Một trực giác ngây thơ có thể gợi ý việc liên tục phân nhóm người giàu và người nghèo để cân bằng mọi thứ, nhưng cách tiếp cận đó dễ dàng bị phá vỡ vì nó bỏ qua sự bảo toàn toàn cầu về tổng số tiền và ràng buộc rằng tất cả các thành viên được chọn đều trở nên bình đẳng. 

Một số trường hợp đặc biệt nêu bật khó khăn: 

Nếu tất cả các giá trị đều dưới đây$x$, nhưng tổng điểm trung bình của họ vẫn thấp hơn$x$, thì không có hoạt động nào có thể tạo ra một người giàu có. Ví dụ: 

đầu vào:```
3 10
3 3 3
```Ngay cả việc phân nhóm mọi người cũng cho điểm trung bình là 3, vì vậy câu trả lời là 0. 

Nếu tồn tại một giá trị rất lớn, có thể tối ưu là chia nó ra cho nhiều người chứ không phải tập trung nó lại. 

Những ví dụ này gợi ý rằng khó khăn chính là chọn một tập hợp con có mức trung bình có thể được đẩy lên trên$x$cho càng nhiều người càng tốt. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là thử mọi nhóm người có thể có và mô phỏng động lực hoạt động. Đối với mỗi tập hợp con, chúng ta có thể tưởng tượng việc phân phối lại tiền và kiểm tra xem có bao nhiêu người có thể tiếp cận ít nhất$x$. Tuy nhiên, đây là số mũ về số lượng tập hợp con và mỗi mô phỏng là tuyến tính, dẫn đến một kết quả không thể thực hiện được.$O(2^n \cdot n)$sự phức tạp. 

Quan sát quan trọng là các phép toán luôn thay thế tập hợp con đã chọn bằng giá trị trung bình của nó. Vì vậy, giá trị duy nhất quan trọng là giá trị trung bình của các nhóm được chọn và cấu hình cuối cùng tương đương với việc chia mọi người thành các nhóm có giá trị bằng giá trị trung bình của nhóm. 

Bây giờ hãy xem xét điều chúng ta muốn: tối đa hóa số lượng người có giá trị cuối cùng ít nhất là$x$. Nếu một nhóm kết thúc với giá trị ít nhất$x$, thì tổng của nó phải ít nhất gấp lần kích thước nhóm$x$. Điều này cho thấy rằng những người có giá trị cao có thể được hình thành bằng cách tập hợp những người dư thừa từ những người khác. 

Một cách tiêu chuẩn để nghĩ về điều này là sắp xếp mảng theo thứ tự giảm dần. Giả sử chúng ta quyết định rằng đỉnh$k$mọi người sẽ trở nên giàu có. Để hỗ trợ điều này, chúng tôi cố gắng “tài trợ” cho họ bằng cách sử dụng các giá trị lớn nhất hiện có. Chiến lược tốt nhất là tham lam: lấy giá trị lớn nhất trước và kiểm tra xem tổng tích lũy của chúng có đủ để duy trì ít nhất không$k \cdot x$. 

Điều này biến vấn đề thành việc kiểm tra, đối với từng tiền tố của các giá trị được sắp xếp, liệu có thể tạo ít nhất tất cả tiền tố đó hay không$x$. Vì giá trị cao hơn giúp đáp ứng yêu cầu nên việc sắp xếp đảm bảo chúng tôi luôn xem xét các ứng viên có lợi nhất trước tiên. 

Chúng tôi tính tổng tiền tố từ lớn nhất đến nhỏ nhất và tìm số tiền lớn nhất$k$sao cho tổng lớn nhất$k$ít nhất là các phần tử$k \cdot x$. Cái đó$k$là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự giảm dần để ưu tiên người có mức tiết kiệm cao nhất. Điều này đảm bảo chúng tôi luôn kiểm tra những ứng viên có triển vọng nhất trước tiên. 
2. Tính tổng tiền tố đang chạy trên mảng đã được sắp xếp. Ở bước$k$, số tiền này đại diện cho tổng tài sản sẵn có trong số những người đứng đầu$k$người giàu nhất. 
3. Đối với mỗi$k$từ 1 đến$n$, kiểm tra xem điều kiện$$\text{prefixSum}(k) \ge k \cdot x$$nắm giữ. Điều này kiểm tra xem liệu chúng ta có thể phân phối lại tổng tài sản đủ để mang lại tất cả$k$ít nhất là những người được chọn$x$. 
4. Theo dõi lớn nhất$k$đó thỏa mãn điều kiện. Giá trị này là số lượng người giàu có tối đa có thể đạt được. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ cấu hình cuối cùng nào tạo nên$k$người giàu phải có tổng tài sản ở những nơi đó$k$vị trí của ít nhất$k \cdot x$. Vì các hoạt động bảo toàn tổng số tiền nên không có sự chuyển đổi nào có thể làm tăng của cải sẵn có trong bất kỳ nhóm được chọn nào. 

Việc sắp xếp đảm bảo chúng ta luôn chọn được những ứng viên mạnh nhất để thành lập một nhóm như vậy. Nếu thậm chí đứng đầu$k$những người giàu nhất không thể thỏa mãn điều kiện, thì không có tập hợp con nào khác có kích thước$k$có thể, vì việc thay thế bất kỳ phần tử nào được chọn bằng phần tử nhỏ hơn chỉ làm giảm tổng số tiền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        a.sort(reverse=True)
        
        total = 0
        ans = 0
        
        for i in range(n):
            total += a[i]
            k = i + 1
            if total >= k * x:
                ans = k
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sắp xếp mảng theo thứ tự giảm dần để chúng tôi đánh giá các ứng cử viên mạnh nhất trước. Tổng tiền tố`total`theo dõi sự giàu có tích lũy của phân khúc hàng đầu hiện tại. Ở mỗi bước, chúng tôi so sánh nó với ngưỡng yêu cầu$k \cdot x$. Biến`ans`giữ độ dài tiền tố hợp lệ tốt nhất được tìm thấy. 

Một điểm tinh tế là chúng ta không bao giờ cần mô phỏng rõ ràng việc phân phối lại. Các phép toán chỉ quan trọng trong chừng mực chúng bảo toàn tổng số tiền và cho phép phân phối lại tùy ý trong một nhóm đã chọn, được nắm bắt hoàn toàn bởi điều kiện tổng tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
5 1 2 1
```Đã sắp xếp:```
[5, 2, 1, 1]
```| k | Tiền tố được chọn | Tổng hợp | k·x | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | [5] | 5 | 3 | vâng | 
| 2 | [5,2] | 7 | 6 | vâng | 
| 3 | [5,2,1] | 8 | 9 | không | 
| 4 | [5,2,1,1] | 9 | 12 | không | 

Câu trả lời là 2. 

Điều này cho thấy chỉ tiền tố mạnh nhất mới có thể duy trì đủ mức độ giàu có trung bình cho mỗi người. 

### Ví dụ 2 

đầu vào:```
4 10
11 9 11 9
```Đã sắp xếp:```
[11, 11, 9, 9]
```| k | Tổng hợp | k·x | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | 11 | 10 | vâng | 
| 2 | 22 | 20 | vâng | 
| 3 | 31 | 30 | vâng | 
| 4 | 40 | 40 | vâng | 

Đáp án là 4. 

Điều này thể hiện trường hợp phân phối lại cho phép cân bằng hoàn toàn ở ngưỡng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp chiếm ưu thế trong mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$thêm | sắp xếp tại chỗ và tích lũy tiền tố | 

Tổng cộng$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^5$, do đó việc sắp xếp tất cả các mảng vẫn hiệu quả trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n, x = map(int, sys.stdin.readline().split())
        a = list(map(int, sys.stdin.readline().split()))
        a.sort(reverse=True)

        total = 0
        ans = 0
        for i, v in enumerate(a):
            total += v
            if total >= (i + 1) * x:
                ans = i + 1
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("4\n4 3\n5 1 2 1\n4 10\n11 9 11 9\n2 5\n4 3\n3 7\n9 4 9\n") == "2\n4\n0\n3"

# custom cases
assert run("1\n1 5\n5\n") == "1", "single element already wealthy"
assert run("1\n3 10\n3 3 3\n") == "0", "cannot reach threshold"
assert run("1\n5 5\n10 1 1 1 1\n") == "2", "one rich supports limited others"
assert run("1\n4 4\n4 4 4 4\n") == "4", "all exactly at threshold"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp tối thiểu | 
| tất cả đều nhỏ | 0 | ngưỡng không thể | 
| một lớn | 2 | hạn chế phân phối lại | 
| giá trị thống nhất | 4 | ranh giới bình đẳng | 

## Vỏ cạnh 

Đối với một người độc thân$a_1 \ge x$, thuật toán ngay lập tức tính nó là hợp lệ vì tổng tiền tố bằng với yêu cầu. 

Đối với đầu vào có tất cả các giá trị bên dưới$x$, điều kiện tiền tố không thành công với mọi$k$, vì tổng số tiền không thể đạt tới$k \cdot x$. Việc sắp xếp không làm thay đổi kết quả này. 

Đối với các đầu vào có độ lệch cao như một giá trị lớn và nhiều giá trị nhỏ, kiểm tra tiền tố sẽ giới hạn chính xác số lượng người có thể được hỗ trợ, bởi vì mỗi người bổ sung sẽ tăng tuyến tính ngưỡng yêu cầu trong khi chỉ đóng góp một khoản tiền biên nhỏ.
