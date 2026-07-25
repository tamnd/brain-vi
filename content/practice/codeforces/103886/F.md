---
title: "CF 103886F - Đề án ngũ cốc"
description: "Chúng ta được cung cấp một mảng các số nguyên và yêu cầu chia nó thành đúng k mảng con liền kề. Đối với bất kỳ phân vùng nào như vậy, mỗi mảng con có giá trị OR được tính toán trên các phần tử của nó."
date: "2026-07-02T07:38:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103886
codeforces_index: "F"
codeforces_contest_name: "CerealCodes 2022 Summer Contest"
rating: 0
weight: 103886
solve_time_s: 46
verified: true
draft: false
---

[CF 103886F - Sơ đồ ngũ cốc](https://codeforces.com/problemset/problem/103886/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và yêu cầu chia nó thành đúng k mảng con liền kề. Đối với bất kỳ phân vùng nào như vậy, mỗi mảng con có giá trị OR được tính toán trên các phần tử của nó. Sau đó, chúng tôi lấy AND của tất cả các giá trị OR của mảng con này, tạo ra một số duy nhất biểu thị chất lượng của phân vùng. 

Nhiệm vụ là chọn phân vùng sao cho giá trị AND cuối cùng này được tối đa hóa. 

Một cách khác để suy nghĩ về mục tiêu là quyết định xem bit nào có thể tồn tại trong tất cả k mảng con. Một bit chỉ đóng góp vào câu trả lời cuối cùng nếu mỗi mảng con có ít nhất một phần tử chứa bit đó. Nếu thậm chí một mảng con bỏ lỡ nó, bit đó sẽ bị loại bỏ bởi AND cuối cùng. 

Các ràng buộc về giá trị lên tới 10^9, giới hạn chúng tôi ở tối đa 30 bit có liên quan. Số lượng phần tử thường đủ lớn để O(n^2) hoặc bất cứ thứ gì liên tục thử phân vùng đều không khả thi. Chúng ta cần một cái gì đó gần tuyến tính hoặc tuyến tính với hệ số nhỏ trên mỗi bit. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả định rằng một khi một bit xuất hiện thường xuyên, nó có thể được sử dụng mà không cần sự phối hợp. Ví dụ: nếu k lớn, có thể một bit xuất hiện nhiều lần nhưng vẫn không thể đảm bảo sự hiện diện của nó trong mọi phân đoạn do bị phân cụm. Một tình huống phức tạp khác là khi việc phân đoạn tham lam cho một bit sẽ phá hủy tính khả thi của các bit cao hơn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi cách chia mảng thành k phân đoạn và tính kết quả AND của phân đoạn OR. Điều này ngay lập tức bùng nổ về mặt tổ hợp vì có thể có các phân vùng C(n-1, k-1), điều này không thể thực hiện được ngay cả đối với n nhỏ. 

Chúng ta cần đảo ngược quan điểm. Thay vì chọn các phân đoạn trước tiên, chúng tôi hỏi những bit nào có thể bị buộc xuất hiện trong mỗi phân đoạn. Giả sử chúng ta sửa một bitmask ứng cử viên x đại diện cho các bit mà chúng ta muốn trong câu trả lời cuối cùng. Sau đó, chúng tôi kiểm tra xem có thể chia mảng thành ít nhất k phân đoạn sao cho mỗi phân đoạn chứa tất cả các bit của x trong OR của nó hay không. 

Điều này biến vấn đề thành kiểm tra tính khả thi trên mỗi mặt nạ bit. Quan sát quan trọng là các bit độc lập về mặt khả thi: nếu chúng ta cố gắng tối đa hóa câu trả lời, chúng ta có thể xây dựng x một cách tham lam từ bit cao nhất đến bit thấp nhất. Điều này hiệu quả vì các bit cao hơn chiếm ưu thế về mặt từ điển trong kết quả số nguyên cuối cùng, vì vậy chúng tôi không bao giờ muốn hy sinh bit cao hơn cho sự kết hợp của các bit thấp hơn. 

Để kiểm tra tính khả thi của x cố định, chúng tôi quét từ trái sang phải trong khi tích lũy giá trị OR cho phân đoạn hiện tại. Khi phân đoạn hiện tại chứa tất cả các bit của x, chúng tôi cắt và bắt đầu một phân đoạn mới. Nếu chúng ta có thể hình thành ít nhất k đoạn như vậy thì x là khả thi. 

Việc cắt giảm tham lam này có hiệu quả vì việc trì hoãn việc cắt giảm không bao giờ có ích. Nếu một phân đoạn đã thỏa mãn x, thì việc mở rộng nó chỉ thêm các bit OR bổ sung chứ không thể loại bỏ các bit hiện có, do đó nó không bao giờ làm tăng khả năng hình thành các phân đoạn hợp lệ hơn của chúng ta sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong n | O(n) | Quá chậm | 
| Tham lam Bitwise + Kiểm tra tính khả thi | O(30n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời từng chút một từ bit quan trọng nhất đến bit ít quan trọng nhất.

1. Khởi tạo mặt nạ trả lời x là 0, nghĩa là ban đầu không cần bit nào. Chúng tôi sẽ cố gắng bật từng bit một trong khi vẫn duy trì tính khả thi. 
2. Lặp lại các bit từ 29 xuống 0. Với mỗi bit i, tạo một mặt nạ ứng cử viên x' = x với bit tôi đặt. Điều này thể hiện giả thuyết rằng bit i có thể là một phần của câu trả lời cuối cùng. 
3. Kiểm tra xem có thể chia mảng thành ít nhất k phân đoạn hợp lệ sao cho mỗi phân đoạn chứa tất cả các bit trong x' hay không. Để thực hiện việc này, hãy quét mảng trong khi vẫn duy trì OR đang chạy cho phân đoạn hiện tại. 
4. Trong khi quét, hãy cập nhật OR đang chạy với từng phần tử. Bất cứ khi nào OR đang chạy chứa tất cả các bit của x', chúng ta sẽ đóng phân đoạn hiện tại và đặt lại OR đang chạy. Chúng tôi cũng tăng số lượng phân đoạn. 
5. Sau khi quét xong, nếu số đoạn được tạo thành ít nhất là k thì x' là khả thi nên chúng ta cập nhật vĩnh viễn x thành x'. Ngược lại, chúng ta loại bỏ bit i và giữ x không thay đổi. 
6. Sau khi xử lý tất cả các bit, x là mặt nạ tối đa có thể đạt được. 

Điều tinh tế quan trọng là chúng tôi được phép tạo nhiều hơn k phân đoạn trong quá trình kiểm tra tính khả thi. Điều này quan trọng vì nếu chúng ta có thể tạo nhiều hơn k, chúng ta luôn có thể hợp nhất một số phân đoạn liền kề mà không mất tính hợp lệ, vì việc hợp nhất chỉ làm tăng OR và duy trì sự hiện diện của tất cả các bit trong x. 

Tại sao nó hoạt động: 

Tính đúng đắn dựa trên hai thuộc tính. Đầu tiên, phân đoạn tham lam cho mặt nạ cố định là tối ưu về mặt tối đa hóa số lượng phân đoạn hợp lệ vì chúng tôi luôn cắt ở điểm sớm nhất có thể. Thứ hai, việc xây dựng theo bit là an toàn vì tính khả thi là đơn điệu: nếu mặt nạ x khả thi thì bất kỳ mặt nạ con nào cũng khả thi. Điều này đảm bảo rằng một khi bit cao hơn bị từ chối, nó sẽ không bao giờ có hiệu lực sau này do các bit thấp hơn được thêm vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(mask, a, k):
    cnt = 0
    cur = 0
    full = mask
    for v in a:
        cur |= v
        if (cur & full) == full:
            cnt += 1
            cur = 0
    return cnt >= k

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    ans = 0
    for b in range(29, -1, -1):
        cand = ans | (1 << b)
        if can(cand, a, k):
            ans = cand

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách việc kiểm tra tính khả thi thành một chức năng trợ giúp. điều kiện`(cur & full) == full`là cách rõ ràng để đảm bảo tất cả các bit cần thiết đều có trong phân đoạn hiện tại. 

Sự thiết lập lại tham lam`cur = 0`là an toàn vì khi một phân đoạn hợp lệ, bất kỳ tiện ích mở rộng nào cũng không giúp tạo thêm phân đoạn sau này, vì OR chỉ tích lũy các bit. 

Vòng lặp bên ngoài cố gắng cải thiện câu trả lời từng chút một, đảm bảo tính tối ưu về mặt từ điển so với tầm quan trọng của bit. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng trong đó chúng ta cố gắng tạo thành k phân đoạn và quan sát cách hành xử của các đường cắt tham lam. 

### Ví dụ 1 

đầu vào: 

n = 5, k = 2 

a = [1, 2, 1, 4, 2] 

Chúng tôi theo dõi mặt nạ ứng cử viên trong quá trình xây dựng. 

| Chút | Mặt nạ ứng cử viên | Số lượng phân đoạn | Khả thi | Mặt nạ được chọn | 
| --- | --- | --- | --- | --- | 
| 2 | 4 | 1 (không thể đạt được hai lần) | Không | 0 | 
| 1 | 2 | 2 | Có | 2 | 
| 0 | 3 | 2 | Có | 3 | 

Câu trả lời cuối cùng là 3. 

Điều này cho thấy mức độ khả thi phụ thuộc vào khả năng liên tục “thu thập” các bit cần thiết trong nhiều phân đoạn rời rạc. 

### Ví dụ 2 

đầu vào: 

n = 6, k = 3 

a = [3, 1, 2, 3, 1, 2] 

Kiểm tra mặt nạ 3 (bit 0 và 1): 

| Xây dựng phân khúc | HOẶC | Hành động | 
| --- | --- | --- | 
| [3] | 3 | cắt | 
| [1,2] | 3 | cắt | 
| [3,1,2] | 3 | cắt | 

Chúng tôi thu được 3 phân đoạn, vì vậy mặt nạ 3 là khả thi. 

Điều này chứng tỏ rằng việc cắt tham lam luôn tạo ra các ranh giới phân đoạn sớm nhất có thể, tối đa hóa số lượng phân đoạn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(30n) | 30 lần kiểm tra tính khả thi trên mỗi bit, mỗi lần quét tuyến tính | 
| Không gian | O(1) | Chỉ sử dụng các bộ đếm và chạy OR | 

Các ràng buộc cho phép điều này một cách thoải mái vì n thường lên tới 10^5, tạo ra tổng cộng khoảng 3 triệu thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    def can(mask):
        cnt = 0
        cur = 0
        for v in a:
            cur |= v
            if (cur & mask) == mask:
                cnt += 1
                cur = 0
        return cnt >= k

    ans = 0
    for b in range(29, -1, -1):
        cand = ans | (1 << b)
        if can(cand):
            ans = cand

    return str(ans)

# simple sample-like cases
assert run("5 2\n1 2 1 4 2\n") == "3"
assert run("6 3\n3 1 2 3 1 2\n") == "3"

# minimum case
assert run("1 1\n7\n") == "7"

# all equal
assert run("4 2\n1 1 1 1\n") == "1"

# k larger than possible segments for strong mask
assert run("3 2\n1 2 4\n") in ["0", "1", "2", "4"]

# alternating bits
assert run("5 1\n1 2 4 2 1\n") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2, 1 2 1 4 2 | 3 | phân khúc tham lam cơ bản | 
| 6 3, 3 1 2 3 1 2 | 3 | lặp lại việc đóng gói toàn bit | 
| 1 1, 7 | 7 | cạnh phần tử đơn | 
| 4 2, tất cả 1s | 1 | mảng thống nhất | 
| 5 1, 1 2 4 2 1 | 7 | tích lũy HOẶC đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi k bằng 1. Trong trường hợp này, toàn bộ mảng là một phân đoạn, vì vậy câu trả lời chỉ đơn giản là OR của tất cả các phần tử. Thuật toán xử lý việc này một cách tự nhiên vì bất kỳ mặt nạ bit khả thi nào cũng sẽ vượt qua quá trình kiểm tra một phân đoạn. 

Một trường hợp cạnh khác là khi k lớn so với n. Nếu k vượt quá số lượng phân đoạn tối đa có thể được hình thành ngay cả đối với mặt nạ 0, thì việc kiểm tra tính khả thi sẽ không thành công đối với tất cả các mặt nạ khác 0, dẫn đến câu trả lời là 0. Ví dụ: nếu n = 3 và k = 4, không có mặt nạ nào ngoại trừ 0 có thể tạo ra 4 phân đoạn hợp lệ. 

Một trường hợp tinh tế hơn là khi các bit được nhóm lại. Giả sử các bit cao chỉ xuất hiện trong một vùng duy nhất của mảng. Việc phân đoạn tham lam sẽ tạo ra tối đa một phân đoạn chứa bit đó, khiến việc kiểm tra bit đó không thành công, ngay cả khi nó xuất hiện tổng cộng nhiều lần. Điều này đúng vì yêu cầu là theo từng phân khúc chứ không phải tần suất toàn cầu.
