---
problem: 954G
contest_id: 954
problem_index: G
name: "Castle Defense"
contest_name: "Educational Codeforces Round 40 (Rated for Div. 2)"
rating: 2000
tags: ["binary search", "data structures", "greedy", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 106
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a327d26-ccf4-83ec-bcbf-87461eeed768
---

#CF 954G - Phòng thủ lâu đài 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, hai con trỏ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a327d26-ccf4-83ec-bcbf-87461eeed768 

--- 

## Giải pháp 

## Hiểu vấn đề 

Mỗi vị trí trên tường có thể coi là một đoạn cần cường độ bảo vệ. Bạn được sắp xếp ban đầu các cung thủ và mỗi cung thủ không chỉ bảo vệ vị trí của mình mà còn hỗ trợ các vị trí lân cận trong bán kính cố định. Vì vậy, thay vì đóng góp thuần túy cục bộ, mỗi cung thủ sẽ dàn trải ảnh hưởng của mình trên một khoảng đối xứng xung quanh vị trí của mình. 

Đối với mỗi phần của bức tường, sức mạnh phòng thủ của nó là tổng số cung thủ có tầm ảnh hưởng bao trùm nó. Chất lượng của kế hoạch phòng thủ được xác định bởi phần được phòng thủ yếu nhất, nghĩa là chúng tôi quan tâm đến việc tối đa hóa phạm vi bao phủ tối thiểu trên tất cả các vị trí. 

Bạn được phép đặt thêm cung thủ ở bất cứ đâu và mỗi cung thủ mới hoạt động giống hệt như những cung thủ hiện có: nó đóng góp vào tất cả các vị trí trong khoảng cách r. Nhiệm vụ là phân bổ những cung thủ bổ sung này sao cho sau tất cả những đóng góp, phạm vi bao phủ nhỏ nhất trong số tất cả các phần tường càng lớn càng tốt. 

Khó khăn chính đến từ sự tương tác giữa vị trí địa phương và tầm ảnh hưởng rộng rãi. Một cung thủ được thêm vào sẽ ảnh hưởng đến toàn bộ khoảng thời gian, vì vậy mọi quyết định đều có những hậu quả chồng chéo. 

Các ràng buộc đủ lớn đến mức bất kỳ mô phỏng tham lam bậc hai hoặc thậm chí ngây thơ nào cũng không thể thực hiện được ngay lập tức. Với tối đa 500.000 vị trí và k có khả năng rất lớn, bất kỳ phương pháp nào cố gắng mô phỏng từng vị trí một hoặc tính toán lại phạm vi bao phủ từ đầu sẽ vượt quá giới hạn thời gian theo nhiều bậc độ lớn. Điều này buộc chúng ta phải hướng tới các chiến lược sử dụng tính đơn điệu và kiểm tra tính khả thi theo thời gian tuyến tính. 

Một vấn đề tế nhị xuất hiện khi r lớn. Trong trường hợp đó, mỗi cung thủ ảnh hưởng đến gần như toàn bộ bức tường, do đó vấn đề trở thành việc cân bằng tổng thể hơn là điều chỉnh cục bộ. Một trường hợp cạnh khác là r = 0, trong đó mỗi vị trí là độc lập và lời giải trở thành bài toán phân phối lại đơn giản hoàn toàn dựa trên thâm hụt cục bộ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tính toán phạm vi bao phủ ban đầu cho mọi vị trí và sau đó cố gắng mô phỏng vị trí của từng cung thủ bổ sung. Mỗi vị trí thay đổi toàn bộ khoảng kích thước khoảng 2r + 1 và việc cập nhật điều này một cách đơn giản sẽ tốn O(n) cho mỗi cung thủ. Với k lên tới 10^18, điều này thậm chí không thể được khái niệm hóa về mặt tính toán. 

Ngay cả khi chúng tôi cố gắng thông minh hơn và luôn đặt cung thủ ở vị trí tối thiểu hiện tại là nhỏ nhất, chúng tôi vẫn phải đối mặt với cùng một vấn đề về cấu trúc: một vị trí duy nhất ảnh hưởng đến nhiều vị trí và các quyết định trong tương lai phụ thuộc vào các hiệu ứng chồng chéo trong tương lai. 

Quan sát quan trọng là câu trả lời đều đơn điệu. Nếu chúng ta có thể đạt được mức phòng thủ tối thiểu là X thì mọi giá trị nhỏ hơn X cũng có thể đạt được. Điều này cho phép chúng ta tìm kiếm nhị phân câu trả lời cuối cùng. 

Thử thách còn lại là kiểm tra xem mục tiêu X cố định có đạt được hay không. Điều này trở thành vấn đề phân bổ nguồn lực: chúng tôi quét từ trái sang phải và đảm bảo mọi vị trí đều đạt ít nhất X. Nếu một vị trí bị thiếu, chúng tôi buộc phải bố trí đủ cung thủ để khắc phục ngay lập tức, vì không vị trí nào trong tương lai có thể giúp được mà không ảnh hưởng đến các quyết định trước đó. 

Mỗi vị trí có thể được mô hình hóa dưới dạng thêm phạm vi phủ sóng trên một phân đoạn liền kề trong “mảng phủ sóng”, cho phép chúng tôi duy trì hiệu ứng bằng cách sử dụng mảng khác biệt và xử lý mọi thứ theo thời gian tuyến tính. 

Điều này chuyển đổi vấn đề từ “chọn k vị trí trên toàn cầu” thành “tham lam thỏa mãn các ràng buộc từ trái sang phải với các cập nhật phạm vi”, điều này hiệu quả và mang tính quyết định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k · n) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + tính khả thi tham lam | O(n log V) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Trước tiên, chúng tôi tính toán phạm vi bao phủ cơ sở cho từng vị trí bằng cách sử dụng tổng tiền tố, vì mỗi cung thủ đóng góp vào một khoảng cố định. Điều này cung cấp cho chúng tôi sức mạnh ban đầu của mỗi phần trước khi bổ sung thêm cung thủ. 

Sau đó, chúng tôi tìm kiếm câu trả lời nhị phân, ký hiệu là X. 

1. Tính toán mảng bao phủ ban đầu b[i], trong đó b[i] là số lượng cung thủ hiện có bao phủ vị trí i. Điều này được thực hiện bằng cách sử dụng một mảng sai phân trong các khoảng [i - r, i + r]. 
2. Xác định hàm can(X) để kiểm tra xem liệu chúng ta có thể đảm bảo mọi vị trí đều có phạm vi bao phủ ít nhất X bằng cách sử dụng tối đa k cung thủ được thêm vào hay không. 
3. Bên trong can(X), duy trì một mảng đang chạy add[i] thể hiện phạm vi bao phủ bổ sung do các cung thủ đã đặt trước đó đóng góp. Chúng tôi đi qua các vị trí từ trái sang phải. 
4. Tại vị trí i, tính toán phạm vi hiện tại là b[i] cộng với các phần bổ sung tích lũy. Nếu giá trị này ít nhất đã là X thì chúng ta tiếp tục. 
5. Nếu nó ở dưới X, chúng ta buộc phải khắc phục mức thâm hụt ngay lập tức. Đặt cần = X - current[i]. Chúng ta đặt nhu cầu cung thủ ở vị trí i + r. Lựa chọn này rất quan trọng vì nó đẩy mức độ bao phủ càng xa càng tốt, đảm bảo chúng tôi không lãng phí tính linh hoạt trong tương lai. 
6. Mỗi vị trí như vậy sẽ tăng phạm vi bao phủ trên khoảng [i, i + 2r]. Chúng tôi áp dụng điều này bằng cách sử dụng một mảng khác biệt để các bản cập nhật vẫn là O(1). 
7. Tích lũy tổng số cung thủ đã đặt. Nếu vượt quá k thì mục tiêu X không thể đạt được. 
8. Nếu chúng tôi hoàn thành quá trình quét mà không vượt quá k thì X là khả thi. 

Tìm kiếm nhị phân chạy trên một phạm vi từ giá trị tối thiểu ban đầu có thể cho đến giá trị đó cộng với k. 

### Tại sao nó hoạt động 

Bước khả thi tham lam dựa trên một lập luận bắt buộc. Khi chúng ta ở vị trí i, bất kỳ sự thiếu hụt nào ở đó đều phải được sửa chữa bởi một cung thủ nào đó có tầm ảnh hưởng bao trùm i. Trong số tất cả các vị trí có thể, việc chọn vị trí hợp lệ ngoài cùng bên phải sẽ tối đa hóa mức độ phù hợp trong tương lai và không bao giờ làm giảm tính khả thi cho các vị trí sau này. Khi chúng tôi sửa vị trí i, chúng tôi không bao giờ cần phải xem lại vị trí đó, bởi vì tất cả các hoạt động trong tương lai chỉ mở rộng phạm vi bao phủ sang bên phải hoặc không ảnh hưởng đến các chỉ mục đã được xử lý theo cách có hại. Điều này tạo ra một bất biến ổn định từ trái sang phải: mọi vị trí được xử lý đều được đảm bảo đáp ứng mục tiêu vĩnh viễn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_base(n, r, a):
    diff = [0] * (n + 2)
    for i, val in enumerate(a):
        if val == 0:
            continue
        l = max(0, i - r)
        rr = min(n - 1, i + r)
        diff[l] += val
        diff[rr + 1] -= val

    b = [0] * n
    cur = 0
    for i in range(n):
        cur += diff[i]
        b[i] = cur
    return b

def can(X, n, r, k, b):
    diff = [0] * (n + 3)
    active = 0
    used = 0

    for i in range(n):
        active += diff[i]
        cur = b[i] + active

        if cur < X:
            need = X - cur
            used += need
            if used > k:
                return False

            active += need
            end = i + 2 * r + 1
            if end < n:
                diff[end] -= need

    return True

def solve():
    n, r, k = map(int, input().split())
    a = list(map(int, input().split()))

    b = build_base(n, r, a)

    lo = min(b)
    hi = lo + k

    while lo < hi:
        mid = (lo + hi + 1) // 2
        if can(mid, n, r, k, b):
            lo = mid
        else:
            hi = mid - 1

    print(lo)

if __name__ == "__main__":
    solve()
```Tính toán cơ sở biến mỗi cung thủ thành một bản cập nhật phạm vi trong khoảng thời gian ảnh hưởng của nó, tạo ra phạm vi bao phủ ban đầu theo thời gian tuyến tính. Kiểm tra tính khả thi sử dụng mảng khác biệt thứ hai, nhưng lần này dành cho các cung thủ được thêm động trong quá trình quét tham lam. 

Sự tinh tế quan trọng là vị trí ở vị trí i + r. Điều này đảm bảo rằng cung thủ được bổ sung bao phủ vị trí i trong khi tối đa hóa phạm vi tiếp cận bên phải, điều này rất cần thiết để giữ cho các quyết định trong tương lai có tính khả thi. 

Tìm kiếm nhị phân kết thúc việc kiểm tra tính khả thi này, khai thác cấu trúc đơn điệu của vấn đề. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
n = 5, r = 0, k = 6
a = [5, 4, 3, 4, 9]
```Vì r = 0, mỗi cung thủ chỉ ảnh hưởng đến vị trí của chính mình, do đó phạm vi bao phủ căn cứ giống hệt với mảng. 

Chúng tôi kiểm tra tính khả thi cho X = 5. 

| tôi | cơ sở b[i] | hoạt động thêm | hiện tại | hành động | đã qua sử dụng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 5 | được | 0 | 
| 1 | 4 | 0 | 4 | thêm 1 | 1 | 
| 2 | 3 | 1 | 4 | được | 1 | 
| 3 | 4 | 1 | 5 | được | 1 | 
| 4 | 9 | 1 | 10 | được | 1 | 

Tại i = 1, ta cần thêm 1 cung thủ. Vì r = 0 nên điều này chỉ cố định vị trí 1. Quá trình tiếp tục mà không vượt quá k nên X = 5 là khả thi. 

Việc thử X = 6 sẽ ngay lập tức thất bại ở vị trí 2 vì mức thâm hụt sẽ vượt quá k khả dụng. 

Dấu vết này cho thấy rằng khi r = 0, bài toán suy biến thành cân bằng độc lập trên mỗi chỉ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | tìm kiếm nhị phân qua câu trả lời, mỗi kiểm tra tính khả thi là tuyến tính | 
| Không gian | O(n) | mảng khác biệt cho mô phỏng cơ bản và tham lam | 

Quá trình quét tuyến tính đủ hiệu quả cho 500.000 vị trí và hệ số logarit vẫn nhỏ vì phạm vi câu trả lời bị giới hạn bởi phạm vi ban đầu cộng với k. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import math

    def build_base(n, r, a):
        diff = [0] * (n + 2)
        for i, val in enumerate(a):
            if val == 0:
                continue
            l = max(0, i - r)
            rr = min(n - 1, i + r)
            diff[l] += val
            diff[rr + 1] -= val

        b = [0] * n
        cur = 0
        for i in range(n):
            cur += diff[i]
            b[i] = cur
        return b

    def can(X, n, r, k, b):
        diff = [0] * (n + 3)
        active = 0
        used = 0
        for i in range(n):
            active += diff[i]
            cur = b[i] + active
            if cur < X:
                need = X - cur
                used += need
                if used > k:
                    return False
                active += need
                end = i + 2 * r + 1
                if end < n:
                    diff[end] -= need
        return True

    n, r, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = build_base(n, r, a)

    lo = min(b)
    hi = lo + k
    while lo < hi:
        mid = (lo + hi + 1) // 2
        if can(mid, n, r, k, b):
            lo = mid
        else:
            hi = mid - 1
    return str(lo)

# provided sample
assert run("5 0 6\n5 4 3 4 9\n") == "5"

# custom cases
assert run("1 0 10\n0\n") == "10", "single cell"
assert run("3 1 0\n1 1 1\n") == "1", "no extra archers"
assert run("5 2 3\n0 0 0 0 0\n") == "1", "pure placement"
assert run("6 0 5\n1 2 3 4 5 6\n") == "3", "independent cells"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 10 | phạm vi cực xa và sử dụng k đầy đủ | 
| không có thêm cung thủ | 1 | tính khả thi mà không cần bổ sung | 
| tất cả đều bằng không | 1 | trường hợp xây dựng thuần túy | 
| mảng tăng dần | 3 | ràng buộc độc lập | 

## Vỏ cạnh 

Khi r = 0, mỗi vị trí hoạt động độc lập vì không có cung thủ nào ảnh hưởng đến các vị trí lân cận. Thuật toán vẫn hoạt động vì mỗi thâm hụt được giải quyết cục bộ và quy trình tham lam không bao giờ lan rộng ảnh hưởng ra ngoài cùng một chỉ mục. Ví dụ: tại vị trí i có b[i] = 0 và mục tiêu X = 3, chúng ta tiêu trực tiếp 3 đơn vị tại vị trí đó mà không ảnh hưởng đến vị trí khác. 

Khi r lớn, gần n, mỗi cung thủ được thêm vào sẽ ảnh hưởng gần như toàn bộ mảng. Trong trường hợp này, việc đặt tất cả các phép cộng ở bất kỳ vị trí tối ưu nào sẽ làm tăng mức tối thiểu toàn cục một cách đồng đều một cách hiệu quả. Quá trình quét tham lam vẫn hoạt động chính xác vì mọi vị trí đều ảnh hưởng như nhau đến tất cả các vị trí còn lại, do đó, các quyết định sớm vẫn có hiệu lực đối với các chỉ mục sau này. 

Khi k = 0, tìm kiếm nhị phân sẽ thu gọn ngay lập tức về mức tối thiểu của mảng bao phủ ban đầu và việc kiểm tra tính khả thi không thực hiện cập nhật nào, phản ánh chính xác rằng không thể cải thiện được.
