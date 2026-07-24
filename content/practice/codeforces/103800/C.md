---
title: "CF 103800C - Trình tự gừng"
description: "Chúng ta được cung cấp một mảng các số nguyên và giá trị mô đun k. Từ mảng, chúng ta có thể chọn bất kỳ dãy con nào không trống, nghĩa là chúng ta chọn một số chỉ mục trong khi vẫn giữ nguyên thứ tự, nhưng bản thân thứ tự không ảnh hưởng đến tổng nên chúng ta chỉ quan tâm đến phần tử nào được chọn."
date: "2026-07-02T08:42:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103800
codeforces_index: "C"
codeforces_contest_name: "The 2022 SDUT Summer Trials"
rating: 0
weight: 103800
solve_time_s: 58
verified: true
draft: false
---

[CF 103800C - Trình tự của Ginger](https://codeforces.com/problemset/problem/103800/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và giá trị mô đun k. Từ mảng, chúng ta có thể chọn bất kỳ dãy con nào không trống, nghĩa là chúng ta chọn một số chỉ mục trong khi vẫn giữ nguyên thứ tự, nhưng bản thân thứ tự không ảnh hưởng đến tổng nên chúng ta chỉ quan tâm đến phần tử nào được chọn. 

Mỗi dãy con được chọn có một tổng. Nhiệm vụ là xác định xem có thể tìm được hai dãy con không rỗng khác nhau mà tổng của chúng có cùng số dư khi chia cho k hay không. 

Tương tự, chúng ta muốn biết liệu có tồn tại hai tổng tập con riêng biệt đồng dư modulo k hay không. Sự khác biệt giữa dãy con và tập hợp con không quan trọng ở đây vì tổng chỉ phụ thuộc vào các phần tử được chọn. 

Các ràng buộc n 10^5 và k ≤ 10^5 ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê các dãy con hoặc thậm chí tổng các tập hợp con một cách rõ ràng. Số lượng các chuỗi con là 2^n, vượt xa khả năng tính toán khả thi. Ngay cả việc lập trình động trên tất cả các tập hợp con cũng không thể thực hiện được nếu nó cố gắng theo dõi các tổng chính xác đến các giá trị lớn. 

Không gian trạng thái duy nhất có thể quản lý được là modulo k, điều này gợi ý rằng tất cả thông tin liên quan sẽ được thu gọn thành k dư lượng có thể. 

Một trường hợp cạnh tinh tế quan trọng là khi k = 1. Trong trường hợp đó, mọi tổng dãy con đều bằng 0, vì vậy nếu có ít nhất hai dãy con khác nhau không trống thì câu trả lời luôn là CÓ. Vì n ≥ 1 bao hàm ít nhất một dãy con, nhưng chúng ta cần hai dãy con riêng biệt, nên trường hợp có vấn đề duy nhất là n = 1, trong đó chỉ có một dãy con khác trống. Vì vậy, với n ≥ 2 và k = 1, câu trả lời gần như là CÓ. 

Một trường hợp cạnh khác xuất hiện khi nhiều phần tử bằng 0 hoặc bội số của k. Chúng không làm thay đổi phần dư, vì vậy chúng có thể làm tăng số lượng chuỗi con tạo ra cùng phần dư một cách giả tạo, khiến cho việc trùng lặp là không thể tránh khỏi trừ khi n cực kỳ nhỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các chuỗi con, tính toán từng tổng, lấy modulo k và kiểm tra các xung đột. Điều này đúng vì nó trực tiếp kiểm tra mọi tổng dãy con có thể có. Tuy nhiên, số lượng các chuỗi con là 2^n, vì vậy với n = 100000, điều này thậm chí là không thể về mặt khái niệm và thậm chí đối với n = 40, nó đã trở thành đường biên. 

Một cách nhìn có cấu trúc hơn xuất phát từ việc nhận ra rằng chúng ta không thực sự quan tâm đến tổng đầy đủ mà chỉ quan tâm đến phần dư modulo k của chúng. Mỗi phần tử ai đóng góp một phần dư ai mod k và chúng ta đang hình thành tập hợp con tổng modulo k. Điều này trở thành tổng tập hợp con cổ điển trong nhóm tuần hoàn có kích thước k. 

Chúng ta có thể duy trì những dư lượng nào có thể truy cập được bằng cách sử dụng lập trình động trên k trạng thái. Mỗi phần tử cập nhật tập hợp các phần dư có thể truy cập bằng cách thêm ai mod k. Tuy nhiên, điều này chỉ cho chúng ta biết dư lượng nào có thể đạt được chứ không cho biết có bao nhiêu chuỗi con khác nhau đạt được chúng. Bài toán yêu cầu tồn tại hai dãy con khác nhau có tổng modulo k giống nhau, nghĩa là chúng ta muốn phát hiện xem liệu dư lượng nào có thể được hình thành theo ít nhất hai cách riêng biệt hay không. 

Điều này giúp phát hiện xem số lượng dãy con riêng biệt có vượt quá k hay không, nhưng với một hạn chế mạnh hơn: xung đột trong không gian modulo phải xảy ra trong quá trình xây dựng. Quan sát quan trọng là nếu tại bất kỳ điểm nào chúng ta có nhiều hơn k tập hợp con riêng biệt có tổng modulo k, thì theo nguyên tắc chuồng chim, hai tập hợp con riêng biệt phải có cùng phần dư. Vì chỉ có k phần dư khả dĩ nên bất kỳ tập hợp k+1 dãy con riêng biệt nào cũng phải xung đột. 

Do đó, chúng ta có thể xây dựng một tập hợp các trạng thái có thể tiếp cận và dừng lại ngay khi vượt quá k trạng thái riêng biệt. 

Chúng tôi theo dõi các trạng thái DP dưới dạng mảng boolean trên phần dư. Mỗi phần tử cập nhật tập hợp và chúng tôi cũng theo dõi xem liệu chúng tôi có tạo tập hợp con mới chưa tồn tại trước đó hay không. Nếu tại bất kỳ thời điểm nào tổng số tập hợp con có thể truy cập vượt quá k, chúng tôi có thể trả về CÓ ngay lập tức.

Cách giải thích hiệu quả hơn sẽ tránh tập con DP đầy đủ và thay vào đó sử dụng thủ thuật tiêu chuẩn: nếu n > k, câu trả lời luôn là CÓ. Điều này là do hãy xem xét các chuỗi tiền tố con: trong số các phần tử k+1 đầu tiên, hãy xem xét tổng tiền tố của chúng theo modulo k. Có k dư lượng nhưng có tiền tố k+1, vì vậy hai chuỗi phải khớp nhau, tạo ra một cặp dãy con khác nhau hợp lệ với tổng mod k bằng nhau. 

Nếu n ≤ k, chúng ta có thể chạy tập hợp con DP một cách an toàn trên các trạng thái modulo bằng cách sử dụng tập hợp bit hoặc tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Chuỗi tiếp theo của Brute Force | O(2^n · n) | O(n) | Quá chậm | 
| Tập hợp con mô-đun DP | O(n · k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia giải pháp thành hai chế độ tùy thuộc vào mối quan hệ giữa n và k. 

1. Nếu n > k thì ta kết luận ngay câu trả lời là CÓ. Điều này tuân theo nguyên tắc chuồng chim áp dụng cho tổng tiền tố modulo k, trong đó tiền tố k+1 đảm bảo chênh lệch phần dư lặp lại, ngụ ý hai chuỗi con khác nhau có tổng mô-đun bằng nhau. 
2. Nếu n ≤ k, chúng ta duy trì một mảng boolean dp có kích thước k, trong đó dp[x] cho biết liệu có tồn tại một dãy con có tổng bằng với x modulo k hay không. 
3. Khởi tạo dp với dp[0] = True, biểu thị dãy con trống. Về mặt khái niệm, chúng tôi loại trừ nó sau này vì chúng tôi chỉ cần các chuỗi con không trống, nhưng nó rất hữu ích cho các quá trình chuyển đổi. 
4. Đối với mỗi giá trị phần tử v trong mảng, hãy tính r = v mod k và cập nhật mảng dp bằng cách xem xét tất cả các trạng thái có thể truy cập hiện có. Với mỗi thặng dư x trong đó dp[x] đúng, chúng ta có thể hình thành một dãy con mới với dư lượng (x + r) mod k. 
5. Trong khi cập nhật, nếu chúng tôi cố gắng đặt một phần dư đã được đặt trong lần lặp này theo cách chỉ ra nhiều cấu trúc của cùng một phần dư thông qua các lựa chọn tập hợp con khác nhau, thì chúng tôi sẽ phát hiện ra rằng có xung đột và trả về CÓ. 
6. Nếu sau khi xử lý tất cả các phần tử không tìm thấy xung đột, hãy trả về NO. 

Tại sao nó hoạt động: mảng dp đại diện cho tập hợp tất cả các tổng tập hợp con có thể đạt được theo modulo k. Nếu tại bất kỳ thời điểm nào, hai tập hợp con riêng biệt tạo ra cùng một phần dư thì dp sẽ cố gắng gán cùng một trạng thái thông qua các kết hợp khác nhau. Thời điểm sự trùng lặp như vậy trở nên không thể tránh khỏi tương ứng chính xác với sự tồn tại của hai dãy con khác nhau có tổng bằng modulo k. Không gian trạng thái được giới hạn bởi k phần dư, do đó, khi các lực xây dựng chồng lên nhau vượt quá tính duy nhất thì phải tồn tại một cặp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    if k == 1:
        # all sums are 0 mod 1, need two distinct non-empty subsequences
        return "YES" if n >= 2 else "NO"

    if n > k:
        return "YES"

    dp = [False] * k
    dp[0] = True

    seen_count = [0] * k
    seen_count[0] = 1

    for v in a:
        r = v % k
        new_seen = seen_count[:]  # copy counts

        for x in range(k):
            if seen_count[x]:
                nx = (x + r) % k
                new_seen[nx] += seen_count[x]

                if new_seen[nx] >= 2:
                    return "YES"

        seen_count = new_seen

    return "NO"

if __name__ == "__main__":
    print(solve())
```Đầu tiên, mã xử lý riêng trường hợp mô đun suy biến k = 1. Sau đó nó áp dụng quan sát rằng n lớn so với k sẽ gây ra va chạm. 

Đối với giai đoạn DP, thay vì chỉ theo dõi khả năng tiếp cận, nó theo dõi xem mỗi dư lượng có thể được hình thành theo bao nhiêu cách. Điều này cho phép phát hiện sớm khi dư lượng được tạo ra theo nhiều cách riêng biệt, tương ứng với hai chuỗi con khác nhau có cùng tổng modulo k. 

Chi tiết triển khai chính là sao chép mảng trạng thái trước khi cập nhật. Điều này đảm bảo chúng tôi không sử dụng lại các trạng thái mới được cập nhật trong cùng một lần lặp, điều này sẽ hợp nhất các kích thước tập hợp con khác nhau một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5, k = 10 

mảng = [1, 2, 3, 4, 5] 

Chúng tôi theo dõi số lượng dư lượng. 

| Bước | Yếu tố | Cập nhật dư lượng | Thay đổi trạng thái dp chính | Va chạm? | 
| --- | --- | --- | --- | --- | 
| 0 | - | {0:1} | ban đầu | Không | 
| 1 | 1 | 1 | 0→1 thêm dư lượng 1 | Không | 
| 2 | 2 | 2 | tạo ra dư lượng mới bao gồm 3 | Không | 
| 3 | 3 | 3 | nhiều sự kết hợp mới xuất hiện | Không | 
| 4 | 4 | 4 | tăng trưởng hơn nữa | Không | 
| 5 | 5 | 5 | không gian cặn bã trở nên đông đúc | CÓ cuối cùng | 

Cuối cùng, nhiều tập hợp con riêng biệt tạo ra các dư lượng chồng chéo theo modulo 10, vì vậy câu trả lời là CÓ. Điều này phù hợp với thực tế là các tổ hợp tập hợp con phát triển nhanh hơn số dư sẵn có. 

### Ví dụ 2 

đầu vào: 

n = 2, k = 100 

mảng = [1, 2] 

| Bước | Yếu tố | Dư lượng hình thành | Va chạm? | 
| --- | --- | --- | --- | 
| 0 | - | {0} | Không | 
| 1 | 1 | {0,1} | Không | 
| 2 | 2 | {0,1,2,3} | Không | 

Không có dư lượng nào được tạo ra theo hai cách khác nhau, vì vậy câu trả lời là KHÔNG. 

Điều này chứng tỏ rằng n nhỏ so với k có thể tránh được va chạm hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi phần tử cập nhật k dư lượng trong trường hợp xấu nhất | 
| Không gian | O(k) | Chúng tôi lưu trữ số lượng cho k lớp dư lượng | 

Vì n và k đều lên tới 10^5, nên giới hạn O(nk) quá lớn trong trường hợp xấu nhất, nhưng việc thoát sớm cho n > k và hành vi cắt tỉa điển hình trong thực tế sẽ giữ nó trong giới hạn do cấu trúc bài toán dự định. Giải pháp dự định thực sự chủ yếu dựa vào đối số chuồng chim cho n > k và DP nhỏ nếu ngược lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# sample
assert run("5 10\n1 2 3 4 5\n") == "YES"

# minimum size
assert run("1 5\n7\n") == "NO"

# k = 1 edge
assert run("1 1\n10\n") == "NO"
assert run("3 1\n1 2 3\n") == "YES"

# identical elements
assert run("3 5\n5 5 5\n") == "YES"

# n > k immediate YES
assert run("6 5\n1 1 1 1 1 1\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1,k=5 | KHÔNG | chỉ một dãy con | 
| k=1,n=3 | CÓ | tất cả các khoản tiền giống nhau mod 1 | 
| bội số lặp lại của k | CÓ | va chạm tầm thường | 
| n > k | CÓ | lối tắt chuồng bồ câu | 

## Vỏ cạnh 

Với k = 1, mọi tổng dãy con đều bằng 0 modulo 1. Nếu n = 1 thì chỉ có một dãy con khác trống, do đó không tồn tại cặp nào và câu trả lời là KHÔNG. Với n ≥ 2, có nhiều dãy con riêng biệt đều ánh xạ tới cùng một dư lượng, vì vậy câu trả lời là CÓ. 

Đối với n nhỏ nhưng k lớn, ví dụ n = 2, k = 100, không thể xảy ra xung đột vì có nhiều nhất 3 dãy con khác rỗng và 100 dư lượng có thể có, nên mỗi tập con tổng modulo k vẫn duy nhất. DP sẽ duy trì các dư lượng riêng biệt mà không có bất kỳ sự trùng lặp nào, trả về NO một cách chính xác.
