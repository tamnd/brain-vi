---
title: "CF 105381L - Túi đựng những đồng xu bị lãng quên"
description: "Chúng ta được cho một chuỗi các đồng xu được xếp thành một hàng, trong đó đồng xu k có giá trị cố định v[k]. Chúng ta được phép chọn một tập hợp con của những đồng tiền này, nhưng có một hạn chế nghiêm ngặt: chúng ta không thể chọn hai đồng tiền có chỉ số khác nhau đúng một."
date: "2026-06-23T16:09:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105381
codeforces_index: "L"
codeforces_contest_name: "National Yang Ming Chiao Tung University 2024 Team Selection Programming Contest"
rating: 0
weight: 105381
solve_time_s: 60
verified: true
draft: false
---

[CF 105381L - Túi đựng những đồng xu bị lãng quên](https://codeforces.com/problemset/problem/105381/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các đồng xu được xếp thành một hàng, trong đó đồng xu`k`có một giá trị cố định`v[k]`. Chúng ta được phép chọn một tập hợp con của những đồng tiền này, nhưng có một hạn chế nghiêm ngặt: chúng ta không thể chọn hai đồng tiền có chỉ số khác nhau đúng một. Nói cách khác, nếu chúng ta chọn đồng xu`i`, sau đó là tiền xu`i-1`Và`i+1`trở nên bị cấm 

Nhiệm vụ là chọn một tập hợp con các chỉ số tuân theo hạn chế này và tối đa hóa tổng các giá trị đã chọn. Chúng tôi không bắt buộc phải chọn bất kỳ đồng xu nào, vì vậy không chọn gì luôn là một lựa chọn hợp lệ. 

Kích thước đầu vào lên tới một triệu xu, điều này ngay lập tức loại trừ mọi phép liệt kê theo cấp số nhân của các tập hợp con. Ngay cả các giải pháp bậc hai cố gắng xem xét tất cả các cặp hoặc tính toán lại các bài toán con một cách không hiệu quả cũng sẽ không đạt. Giải pháp phải xử lý mảng theo thời gian tuyến tính với công việc không đổi trên mỗi vị trí. 

Một điểm tinh tế là các giá trị có thể âm. Điều này tạo ra những trường hợp lấy xu có hại và chiến lược tối ưu có thể bỏ qua nhiều hoặc thậm chí tất cả xu. Ví dụ: nếu đầu vào là`[-5, -2, -8]`, câu trả lời đúng là`0`, không`-2`hoặc bất kỳ số tiền âm nào, bởi vì không được phép chọn gì và tốt hơn là nhận bất kỳ khoản đóng góp âm nào. 

Một trường hợp cạnh khác đến từ các dấu hiệu xen kẽ. Đối với đầu vào`[10, -1, 10]`, tham lam tích cực không vi phạm tính liền kề, nhưng đối với`[10, 5, 10]`, chọn cả hai số 10 là tối ưu trong khi bỏ qua hoàn toàn phần giữa. Điều này cho thấy sự lựa chọn cục bộ chỉ dựa trên giá trị tức thời là không đủ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là thử tất cả các tập hợp con của các chỉ số và kiểm tra xem có hai chỉ số được chọn nào có liền kề nhau hay không. Đối với mỗi tập hợp con hợp lệ, chúng tôi tính tổng của nó và giữ mức tối đa. Điều này đúng vì nó khám phá toàn bộ không gian tìm kiếm, nhưng số lượng tập hợp con là`2^n`, điều này là không thể ngay cả đối với`n = 40`, huống hồ là`10^6`. 

Chúng ta có thể cải thiện bằng cách chú ý đến cấu trúc của ràng buộc. Mỗi vị trí chỉ xung đột với các vị trí lân cận, có nghĩa là các quyết định tạo thành một chuỗi. Tại chỉ mục`i`, chúng ta chỉ cần biết liệu chúng ta đã lấy`i-1`hoặc bỏ qua nó. Điều này làm giảm sự bùng nổ tổ hợp toàn cầu thành sự tái diễn cục bộ. 

Tại vị trí`i`, chỉ có hai lựa chọn có ý nghĩa: hoặc là chúng ta không lấy xu`i`, trong trường hợp đó chúng tôi sẽ đưa ra giải pháp tốt nhất cho đến`i-1`, hoặc chúng tôi lấy tiền xu`i`, trong trường hợp đó chúng ta phải bỏ qua`i-1`, và chúng tôi thêm`v[i]`đến giải pháp tốt nhất`i-2`. Điều này tạo ra một quá trình chuyển tiếp lập trình động đơn giản trên một dòng. 

Lực lượng vũ phu hoạt động vì nó khám phá rõ ràng tất cả các tập hợp con, nhưng không thành công vì số lượng tập hợp con tăng theo cấp số nhân. Việc quan sát rằng mỗi trạng thái chỉ phụ thuộc vào hai vị trí trước đó cho phép chúng ta nén không gian trạng thái thành một phép truy hồi tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n) | O(n) | Quá chậm | 
| Lập trình động | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải, duy trì tổng tốt nhất có thể đạt được cho mỗi tiền tố trong khi vẫn tôn trọng giới hạn kề. 

1. Chúng tôi xác định`dp[i]`là số tiền tối đa chúng ta có thể nhận được bằng cách sử dụng tiền xu từ chỉ mục`1`ĐẾN`i`. Cách giải thích này đảm bảo chúng tôi chỉ xem xét các tiền tố hợp lệ ở mỗi bước. 
2. Đối với từng vị trí`i`, chúng tôi quyết định giữa việc bỏ qua hoặc lấy xu`i`. Nếu chúng ta bỏ qua nó, giá trị tốt nhất vẫn còn`dp[i-1]`. Điều này tương ứng với việc đưa ra giải pháp tối ưu mà không thay đổi. 
3. Nếu chúng ta lấy đồng xu`i`, chúng tôi buộc phải loại trừ`i-1`, vì vậy điều tốt nhất chúng ta có thể xây dựng là`dp[i-2] + v[i]`. Đây là bước lan truyền ràng buộc chính, trong đó quy tắc kề trực tiếp làm giảm lịch sử có sẵn. 
4. Chúng tôi thiết lập`dp[i] = max(dp[i-1], dp[i-2] + v[i])`. Điều này đảm bảo chúng tôi duy trì tùy chọn hợp lệ tốt nhất ở mọi tiền tố mà không cần phải nhớ cấu trúc tập hợp con đầy đủ. 
5. Để tránh lưu trữ toàn bộ mảng, chúng ta chỉ giữ lại hai trạng thái cuối cùng. Chúng tôi duy trì`prev2 = dp[i-2]`Và`prev1 = dp[i-1]`, cập nhật chúng lặp đi lặp lại khi chúng tôi tiến về phía trước. 

### Tại sao nó hoạt động 

Tại mọi chỉ số`i`, bất kỳ lựa chọn hợp lệ nào trong lần đầu tiên`i`các phần tử phải thuộc đúng một trong hai loại: hoặc nó không bao gồm`i`, hoặc nó bao gồm`i`. Nếu nó không bao gồm`i`, giá trị của nó được giới hạn bởi`dp[i-1]`. Nếu nó bao gồm`i`, thì nó không thể bao gồm`i-1`, do đó phần còn lại của lựa chọn là một giải pháp hợp lệ ở lần đầu tiên`i-2`các phần tử. Hai trường hợp này rời rạc và đầy đủ, đảm bảo rằng việc lấy mức tối đa của chúng sẽ tạo ra giải pháp tối ưu cho tiền tố`i`. Cấu trúc quy nạp này đảm bảo tính chính xác trên toàn bộ mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    v = list(map(int, input().split()))
    
    if n == 0:
        print(0)
        return

    prev2 = 0
    prev1 = 0

    for i in range(n):
        take = prev2 + v[i]
        skip = prev1
        cur = take if take > skip else skip
        prev2 = prev1
        prev1 = cur

    print(prev1)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh sự tái diễn trực tiếp. Biến`prev1`luôn giữ kết quả tốt nhất so với chỉ số trước đó, trong khi`prev2`lưu trữ kết quả trở lại hai bước. Thứ tự cập nhật rất quan trọng:`prev2`phải được cập nhật sau khi tính toán`cur`, nếu không việc lặp lại sẽ vô tình sử dụng lại trạng thái bị ghi đè. 

Việc khởi tạo bằng số 0 ngầm xử lý các giá trị âm một cách chính xác. Vì chúng tôi không cho phép chọn gì nên bắt đầu từ số 0 đảm bảo rằng chúng tôi không bao giờ buộc phải đóng góp tiêu cực. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`v = [3, 2, 5]`. 

Chúng tôi theo dõi`prev2`,`prev1`, Và`cur`: 

| tôi | v[i] | lấy = prev2 + v[i] | bỏ qua = trước1 | cur | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | 0 | 3 | 
| 1 | 2 | 2 | 3 | 3 | 
| 2 | 5 | 8 | 3 | 8 | 

Câu trả lời cuối cùng là`8`, đạt được bằng cách chọn chỉ số`1`Và`3`. Điều này cho thấy việc bỏ qua phần tử trung gian sẽ mang lại lợi ích lớn hơn sau này như thế nào. 

Bây giờ hãy xem xét`v = [4, -1, 2, 10]`. 

| tôi | v[i] | lấy | bỏ qua | cur | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 4 | 0 | 4 | 
| 1 | -1 | -1 | 4 | 4 | 
| 2 | 2 | 6 | 4 | 6 | 
| 3 | 10 | 14 | 6 | 14 | 

Dấu vết này cho thấy cách tránh các giá trị âm một cách tự nhiên trừ khi chúng cho phép kết hợp tốt hơn trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đồng xu đóng góp một lượng công việc không đổi trong quá trình tái phát | 
| Không gian | O(1) | Chỉ có hai trạng thái cuộn được lưu trữ bất kể kích thước đầu vào | 

Việc quét tuyến tính là cần thiết vì mỗi vị trí chỉ phụ thuộc vào các vị trí lân cận và không có cấu trúc nào cho phép bỏ qua các chỉ số một cách an toàn. Với`n`lên tới một triệu, một giải pháp vượt qua duy nhất phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# minimum size
assert run("1\n5\n") == "5"

# all negative values
assert run("3\n-5 -2 -8\n") == "0"

# increasing values
assert run("4\n1 2 3 4\n") == "6"

# alternating high values
assert run("5\n10 1 10 1 10\n") == "30"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 5 | xử lý trường hợp cơ bản | 
| tất cả tiêu cực | 0 | tối ưu lựa chọn trống | 
| ngày càng tăng | 6 | bỏ qua các phần tử ở giữa | 
| mức cao xen kẽ | 30 | tích lũy không liền kề tối ưu | 

## Vỏ cạnh 

Đối với một đồng tiền như`n = 1, v = [7]`, bộ thuật toán`prev1 = 7`sau lần lặp đầu tiên, trực tiếp chọn tùy chọn khả dụng duy nhất. Sự lặp lại tự nhiên giảm xuống thành một so sánh duy nhất giữa lấy và bỏ qua, trong đó bỏ qua mang lại kết quả bằng 0 và lấy mang lại kết quả bảy. 

Đối với đầu vào hoàn toàn âm như`[-3, -1, -2]`, trạng thái lăn đảm bảo rằng mọi`take`giá trị là âm trong khi`skip`mang số 0 về phía trước. Ở mỗi bước, mức tối đa giữ kết quả ở mức 0, mô hình hóa chính xác tùy chọn không chọn đồng xu nào.
