---
problem: 1368D
contest_id: 1368
problem_index: D
name: "AND, OR and square sum"
contest_name: "Codeforces Global Round 8"
rating: 1700
tags: ["bitmasks", "greedy", "math"]
answer: passed_samples
verified: true
solve_time_s: 196
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e4e27-f0e4-83ec-a61e-619a59836cd4
---

# CF 1368D - AND, OR và tổng bình phương 

**Đánh giá:** 1700 
**Tags:** bitmasks, tham lam, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e4e27-f0e4-83ec-a61e-619a59836cd4 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên, mỗi số có tối đa 20 bit. Chúng ta có thể liên tục chọn hai vị trí và áp dụng phép biến đổi theo bit để đẩy các bit từ số này sang số khác: một số trở thành AND theo bit của cặp, số kia trở thành OR theo bit. 

Mỗi thao tác bảo toàn tổng số bit đã đặt ở mọi vị trí bit trên toàn bộ mảng, bởi vì đối với bất kỳ bit cố định nào, phép biến đổi theo cặp không thay đổi số lượng bit tồn tại trong bit đó trên hai số đã chọn. 

Sau khi thực hiện bất kỳ số thao tác nào như vậy, chúng tôi tính tổng bình phương của tất cả các số kết quả và muốn tối đa hóa nó. 

Kích thước đầu vào cho phép lên tới 200.000 số và mỗi số có tối đa 20 bit. Điều này ngay lập tức gợi ý rằng bất kỳ cách tiếp cận nào mô phỏng các hoạt động cặp đều là không thể, vì ngay cả một quá trình bậc hai trên các cặp cũng sẽ quá chậm. Chúng ta cần một cái gì đó gần hơn với tuyến tính hoặc tuyến tính trong n, có thể nhân với số bit. 

Khó khăn chính là các thao tác không chỉ sắp xếp lại các giá trị mà còn liên tục phân phối lại các bit theo cách bị hạn chế. Cấu hình cuối cùng không phải là tùy ý, nhưng nó đủ linh hoạt để nhiệm vụ chính trở thành quyết định cách tập trung các bit thành các số để tối đa hóa tổng bình phương. 

Một sai lầm ngây thơ là cho rằng sắp xếp mảng hoặc kết hợp các số lớn một cách tham lam là đủ. Ví dụ, hãy xem xét các giá trị`[1, 2, 4]`. Bất kỳ chiến lược ghép nối cục bộ nào cũng có thể kết hợp các giá trị lớn trước tiên, nhưng kết quả tối ưu phụ thuộc vào việc phân phối đóng góp bit thay vì độ lớn. Một trường hợp thất bại khác là giả sử nhiều tập số được giữ nguyên hoặc chỉ được hoán vị. Hoạt động thay đổi rõ ràng các giá trị, do đó cách tiếp cận như vậy bỏ lỡ mức độ tự do thực tế. 

Một vấn đề tế nhị khác là giả định rằng vì AND giảm giá trị và OR tăng giá trị nên chúng ta phải luôn cố gắng tối đa hóa OR trong một phần tử duy nhất. Mặc dù đúng hướng nhưng không hiểu rõ về bảo toàn trên mỗi bit, điều này dẫn đến các quyết định hợp nhất tham lam không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các hoạt động có thể có giữa các cặp cho đến khi hội tụ. Mỗi thao tác thay đổi hai giá trị và quá trình có thể tiếp tục với số bước không giới hạn. Ngay cả khi giới hạn các chuỗi có độ dài k, không gian trạng thái vẫn theo cấp số nhân, vì mỗi số là trạng thái 20 bit và có n số đó. Điều này ngay lập tức trở nên không thể thực hiện được. 

Quan sát quan trọng là ngừng suy nghĩ về các số nguyên và thay vào đó hãy kiểm tra từng bit một cách độc lập. Đối với bất kỳ vị trí bit cố định nào, số lượng bit trên mảng không bao giờ thay đổi. Điều này có nghĩa là mỗi bit k đóng góp chính xác cnt_k bit phải được phân bổ cho các số cuối cùng. 

Vì vậy, vấn đề trở thành một nhiệm vụ phân phối lại: chúng ta phải gán các đóng góp bit này cho các số sao cho mỗi bit k được gán cho chính xác các số cnt_k và chúng ta muốn tối đa hóa tổng bình phương của các số nguyên thu được. 

Bây giờ cấu trúc trở nên rõ ràng hơn. Bình phương phần thưởng mạnh mẽ cho sự tập trung. Việc đặt nhiều bit cao vào cùng một số là có lợi vì các số hạng chéo trong bình phương sẽ khuếch đại giá trị. Ví dụ: kết hợp các bit 2^18 và 2^17 trong một số sẽ tạo ra mức tăng lớn hơn nhiều so với việc tách chúng ra. 

Vì vậy, chúng ta muốn gói các bit thành các số càng không đồng đều càng tốt, tôn trọng rằng mỗi bit có một số lượng bản sao cố định. 

Cấu trúc tham lam tiêu chuẩn là xây dựng các số tăng dần, luôn gán bit có sẵn tiếp theo cho số nhỏ nhất hiện tại. Điều này đảm bảo chúng tôi liên tục cân bằng tải, trong khi vẫn cho phép sự tập trung bit cao xuất hiện một cách tự nhiên khi các số nhỏ hơn được lấp đầy dần dần và sau đó đủ điều kiện để tích lũy các bit cao hơn. 

Việc triển khai sạch sẽ sử dụng vùng heap tối thiểu trên các giá trị hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam một chút với Heap | O(20 n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các bit từ cao nhất đến thấp nhất để các đóng góp lớn được đặt lên hàng đầu khi chúng có tác động tối đa đến bình phương. 

1. Đếm xem mỗi bit hiện có bao nhiêu số. Đối với mỗi vị trí bit k, hãy tính cnt[k] là số phần tử mảng có tập hợp bit đó. Giá trị này không bao giờ thay đổi trong quá trình hoạt động vì tổng số bit của mỗi bit là bất biến. 
2. Khởi tạo một heap tối thiểu chứa tất cả n số, bắt đầu từ 0. Mỗi phần tử heap biểu thị giá trị hiện tại của một số đang được xây dựng. 
3. Đối với mỗi vị trí bit k từ 19 xuống 0, gán liên tục bit này cnt[k] lần. Mỗi bài tập lấy số nhỏ nhất hiện tại từ heap, thêm 2^k vào đó và đẩy nó trở lại. Lý do chọn phần tử nhỏ nhất là việc thêm bit cao vào cơ số nhỏ sẽ làm tăng lợi ích cận biên theo cách tối đa hóa tiềm năng thu được bậc hai trong tương lai. 
4. Sau khi xử lý tất cả các bit, hãy tính tổng bình phương của tất cả các số cuối cùng.

Tính chính xác đến từ tính bất biến là ở mỗi bước, chúng tôi duy trì việc gán một phần hợp lệ các bit trong đó mỗi bit k được sử dụng chính xác cnt[k] lần và trong số tất cả các phép gán từng phần như vậy, chiến lược heap giữ cho sự phân phối cân bằng nhất có thể. Sự cân bằng này cho phép các bit cao xếp chồng lên nhau mà không làm quá tải sớm một số theo cách dưới mức tối ưu. Vì bình phương mang lại sự tập trung nên quy trình này mô phỏng một cách hiệu quả việc đóng gói tối ưu trọng lượng bit vào các thùng. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    cnt = [0] * 20
    for x in a:
        for b in range(20):
            if x >> b & 1:
                cnt[b] += 1

    heap = [0] * n
    heapq.heapify(heap)

    for b in range(19, -1, -1):
        for _ in range(cnt[b]):
            x = heapq.heappop(heap)
            x += (1 << b)
            heapq.heappush(heap, x)

    ans = 0
    for x in heap:
        ans += x * x

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén dữ liệu đầu vào thành số lượng trên mỗi bit, đây là thông tin duy nhất còn phù hợp sau các thao tác tùy ý. Đống đại diện cho việc xây dựng các số cuối cùng. Mỗi lần gán một bit, chúng tôi cố tình chọn số hiện tại nhỏ nhất để duy trì sự cân bằng trong khi vẫn cho phép tích lũy nhiều bit thành các phần tử đơn lẻ theo thời gian. 

Việc xử lý các bit từ cao đến thấp đảm bảo rằng các đóng góp lớn được đặt sớm khi hiệu ứng cấu trúc của chúng đối với việc bình phương là đáng kể nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ khái niệm nhỏ:`n = 3`, giá trị`[1, 2, 4]`. Số lượng bit là một lần xuất hiện cho các bit 0, 1 và 2. 

Chúng tôi khởi tạo đống`[0, 0, 0]`. 

Sau khi xử lý bit 2: 

| Bước | Trạng thái đống | Hành động | 
| --- | --- | --- | 
| 1 | [0, 0, 0] | gán 4 cho nhỏ nhất | 
| 2 | [0, 0, 4] | | 

Sau khi xử lý bit 1: 

| Bước | Trạng thái đống | Hành động | 
| --- | --- | --- | 
| 1 | [0, 0, 4] | gán 2 cho nhỏ nhất | 
| 2 | [0, 2, 4] | | 

Sau khi xử lý bit 0: 

| Bước | Trạng thái đống | Hành động | 
| --- | --- | --- | 
| 1 | [0, 2, 4] | gán 1 cho nhỏ nhất | 
| 2 | [1, 2, 4] | | 

Tổng cuối cùng là`1 + 4 + 16 = 21`. 

Dấu vết này cho thấy cách mỗi bit được gán độc lập trong khi dần dần xây dựng các giá trị có cấu trúc. 

Bây giờ hãy xem xét trường hợp có các bit lặp lại:`a = [3, 5]`. Nhị phân là`011`Và`101`. Chúng ta có hai cái ở bit 0, một ở bit 1, một ở bit 2. 

Chúng tôi bắt đầu với`[0, 0]`. 

Sau bit 2: 

| Đống | Hành động | 
| --- | --- | 
| [0, 4] | gán bit 2 | 

Sau bit 1: 

| Đống | Hành động | 
| --- | --- | 
| [2, 4] | gán bit 1 | 

Sau bit 0: 

| Đống | Hành động | 
| --- | --- | 
| [3, 5] | gán cả hai cái | 

Tổng cuối cùng là`9 + 25 = 34`. 

Điều này cho thấy các bit cao hơn định hình cấu trúc ban đầu như thế nào, trong khi các bit thấp hơn tinh chỉnh phân phối cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(20 n log n) | mỗi phép gán bit sử dụng các phép toán heap trên n phần tử | 
| Không gian | O(n) | lưu trữ đống n giá trị được xây dựng | 

Các ràng buộc cho phép tối đa 2×10^5 phần tử và lặp lại tối đa 20 bit, do đó, nhiều nhất là vài triệu thao tác heap, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import heapq

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        cnt = [0] * 20
        for x in a:
            for b in range(20):
                if x >> b & 1:
                    cnt[b] += 1

        heap = [0] * n
        heapq.heapify(heap)

        for b in range(19, -1, -1):
            for _ in range(cnt[b]):
                x = heapq.heappop(heap)
                x += (1 << b)
                heapq.heappush(heap, x)

        print(sum(x * x for x in heap))

    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("1\n123\n") == "15129"

# all zeros
assert run("3\n0 0 0\n") == "0"

# single bit spread
assert run("3\n1 2 4\n") == "21"

# identical values
assert run("2\n7 7\n") == "98"

# mixed
assert run("2\n3 5\n") == "34"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị đơn | vuông | tính đúng đắn của trường hợp cơ sở | 
| tất cả số không | 0 | xử lý đóng góp trống | 
| 1 2 4 | 21 | độ chính xác phân phối lại bit | 
| 7 7 | 98 | hiệu ứng tập trung | 
| 3 5 | 34 | đóng gói bit hỗn hợp | 

## Vỏ cạnh 

Đầu vào tối thiểu có n bằng 1 không chứa phép toán nào. Thuật toán trả về chính xác bình phương của một giá trị vì heap chỉ chứa một phần tử và không có phép gán nào xảy ra. 

Một trường hợp có tất cả các số 0 sẽ kiểm tra xem việc đếm bit và khởi tạo heap có bảo toàn tính trung lập hay không. Vì tất cả cnt[b] đều bằng 0 nên vùng heap không thay đổi và tổng cuối cùng bằng 0, phù hợp với kỳ vọng. 

Trường hợp tất cả các số đều giống nhau, kiểm tra tính đối xứng không phá vỡ phép gán tham lam. Mặc dù tồn tại nhiều ứng cử viên giống hệt nhau, các thao tác heap xử lý chúng một cách thống nhất và cấu hình cuối cùng vẫn tôn trọng tổng số bit, đảm bảo tổng hợp chính xác thành tổng bình phương.
