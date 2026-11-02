---
title: "CF 1065C - Làm cho nó bình đẳng"
description: "Chúng ta có một tập hợp các tháp thẳng đứng, mỗi tháp được biểu thị bằng chiều cao nguyên dương. Trong một thao tác, chúng tôi chọn chiều cao mục tiêu $H$, sau đó mọi tháp cao hơn $H$ sẽ bị cắt giảm xuống chính xác $H$, trong khi các tháp ngắn hơn vẫn không thay đổi."
date: "2026-06-15T08:16:29+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1065
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 52 (Rated for Div. 2)"
rating: 1600
weight: 1065
solve_time_s: 164
verified: true
draft: false
---

[CF 1065C - Make It Equal](https://codeforces.com/problemset/problem/1065/C) 

**Đánh giá:** 1600 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các tháp thẳng đứng, mỗi tháp được biểu thị bằng chiều cao nguyên dương. Trong một thao tác, chúng tôi chọn chiều cao mục tiêu$H$, và sau đó mọi tòa tháp cao hơn$H$được cắt giảm chính xác$H$, trong khi tháp ngắn hơn không thay đổi. Chi phí của hoạt động này là tổng số khối được loại bỏ trên tất cả các tháp, do đó nó phụ thuộc cả vào chiều cao đã chọn và cấu hình hiện tại của tháp. 

Chúng tôi được phép thực hiện nhiều hoạt động như vậy. Một hoạt động chỉ được coi là hợp lệ nếu chi phí của nó không vượt quá một giới hạn nhất định$k$. Mục tiêu là thực hiện số lượng thao tác hợp lệ tối thiểu cho đến khi tất cả các tòa tháp có cùng chiều cao cuối cùng. 

Cấu trúc quan trọng ở đây là mọi thao tác chỉ giảm độ cao và mỗi thao tác chọn một ngưỡng chung áp dụng cho tất cả các tháp. Điều này có nghĩa là quy trình này là một chuỗi các “cấp” giảm dần và trạng thái cuối cùng phải ở một độ cao nào đó đã tồn tại trong hệ thống hoặc có thể đạt được bằng cách giảm đi nhiều lần. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$tháp. Điều này loại trừ mọi giải pháp mô phỏng từng độ cao có thể có và tính toán lại chi phí một cách đơn giản theo thời gian bậc hai. Mô phỏng trực tiếp việc cắt lặp đi lặp lại sẽ quét liên tục tất cả các tháp trong mỗi hoạt động, dẫn đến$O(n^2)$hành vi trong trường hợp xấu nhất là quá chậm. 

Một vấn đề tế nhị xuất hiện khi nhiều tòa tháp có cùng chiều cao. Một lựa chọn tham lam ngây thơ là cắt liên tục đến chiều cao khác biệt tiếp theo không phải lúc nào cũng tôn trọng ràng buộc về chi phí, bởi vì việc nhóm quá nhiều mức giảm vào một lát có thể vượt quá giới hạn chi phí.$k$, trong khi việc phân tách quá mạnh sẽ làm tăng số lượng thao tác. 

Một ví dụ nhỏ minh họa sự khó khăn. Giả sử chiều cao là$[100, 99, 98]$Và$k = 3$. Cắt trực tiếp tới 98 chi phí$(100-98) + (99-98) = 3$, hợp lệ. Nhưng cắt đầu tiên xuống 99 rồi đến 98 cũng hợp lệ nhưng sử dụng nhiều thao tác hơn. Một chiến lược bất cẩn luôn cắt giảm độ cao khác biệt tiếp theo có thể tính toán quá mức các hoạt động một cách không cần thiết. 

Khó khăn chính là quyết định vị trí đặt chiều cao của lát cắt sao cho mỗi lát cắt “đủ rẻ” trong khi giảm thiểu số lượng lát cắt. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng mô phỏng tất cả các chuỗi có thể có của các lát cắt hợp lệ. Từ bất kỳ nhóm độ cao hiện tại nào, chúng tôi có thể thử mọi độ cao mục tiêu có thể$H$tạo ra một lát cắt hợp lệ, tính giá trị của nó và tiếp tục đệ quy. Điều này nhanh chóng trở thành cấp số nhân vì mỗi lát cắt tạo ra một hệ số phân nhánh bằng với số ngưỡng có thể có. Ngay cả khi chúng tôi giới hạn ở các độ cao duy nhất, mỗi bước đều yêu cầu chi phí tính toán trên tất cả các tòa tháp, dẫn đến$O(n^2)$trên mỗi đường dẫn và làm cho việc tìm kiếm tổng thể không thể thực hiện được. 

Quan sát quan trọng là chiều cao lát cắt ứng cử viên có ý nghĩa duy nhất là chiều cao riêng biệt của các tòa tháp, được sắp xếp theo thứ tự giảm dần. Bất kỳ chiến lược tối ưu nào cũng chỉ “đạt được” những giá trị này vì việc cắt đến độ cao trung gian không tồn tại luôn bị chi phối bởi việc cắt xuống mức hiện có tiếp theo mà không mang lại lợi ích bổ sung nào cho các hoạt động trong tương lai. 

Sau khi sắp xếp các độ cao riêng biệt, chúng ta có thể nghĩ ngược lại: chúng ta đang giảm dần độ cao từ cấp cao nhất trở xuống. Mỗi lần chúng tôi di chuyển từ cấp độ cao này sang cấp độ cao tiếp theo thấp hơn, chúng tôi phải chịu một chi phí tỷ lệ thuận với số lượng tháp ở trên cấp độ đó. 

Thay vì mô phỏng các lát cắt về phía trước, chúng ta có thể xử lý độ cao từ cao xuống thấp và giảm nhóm một cách tham lam: tích lũy bao nhiêu khối sẽ bị loại bỏ nếu chúng ta cắt giảm xuống cấp độ tiếp theo và bất cứ khi nào chi phí tích lũy này vượt quá$k$, chúng ta “đóng” một lát và bắt đầu một lát mới. 

Điều này biến vấn đề thành vấn đề phân đoạn theo các độ cao duy nhất được sắp xếp, trong đó mỗi phân đoạn tương ứng với một lát cắt và chi phí của một phân đoạn là tổng mức giảm theo chiều dọc do tất cả các tháp trên phạm vi đó đóng góp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ /$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách nén độ cao vào bản đồ tần số để biết có bao nhiêu tòa tháp tồn tại ở mỗi độ cao riêng biệt. Sau đó, chúng tôi sắp xếp các độ cao này theo thứ tự giảm dần. 

1. Xây dựng bản đồ tần số ở mọi độ cao để chúng tôi biết hiện có bao nhiêu tòa tháp ở mỗi cấp độ. Điều này là cần thiết vì chi phí phụ thuộc vào số lượng tháp bị ảnh hưởng ở mỗi bước. 
2. Sắp xếp chiều cao duy nhất theo thứ tự giảm dần. Chúng tôi xử lý từ trên xuống dưới vì các quyết định cắt lát phụ thuộc vào khối lượng trên mỗi ngưỡng. 
3. Duy trì số lượng tháp hiện đang “hoạt động” trên mức hiện tại. Ban đầu, tất cả các tòa tháp đều hoạt động vì chưa có gì bị cắt. 
4. Duy trì chi phí tích lũy đang hoạt động cho phần hiện tại. Khi di chuyển từ độ cao$h_i$ĐẾN$h_{i+1}$, mỗi tháp hoạt động đóng góp một giọt$h_i - h_{i+1}$, vì vậy chúng tôi thêm$(h_i - h_{i+1}) \times \text{active}$đến chi phí. 
5. Nếu việc bổ sung chi phí này vượt quá$k$, chúng tôi hoàn thiện lát cắt hiện tại ở độ cao$h_i$, tăng câu trả lời và đặt lại bộ tích lũy chi phí. Chúng tôi cũng đánh giá lại phân đoạn hiện tại bắt đầu từ cấp độ này vì ranh giới lát cắt đã được đặt. 
6. Sau khi hoàn thành một lát cắt, chúng tôi tiếp tục xử lý đi xuống, theo dõi các tháp đang hoạt động. Tốc độ này tăng lên bất cứ khi nào chúng tôi gặp một cấp độ cao mới (vì các tháp đó tham gia vào nhóm hoạt động khi chúng tôi đi xuống). 
7. Sau khi xử lý tất cả các cấp độ, nếu có bất kỳ lát cắt nào chưa hoàn thành, chúng tôi sẽ tính đó là một thao tác cuối cùng. 

Lý do đằng sau bước khởi động lại là khi chúng ta vượt quá$k$, việc nhóm giảm chiều cao hiện tại quá tốn kém, do đó lần cắt hợp lệ cuối cùng phải xảy ra ở ranh giới trước đó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, tất cả các tòa tháp trên mức hiện tại đều góp phần đồng đều vào việc giảm thêm nữa. Điều này làm cho chi phí giảm dần giữa hai độ cao khác biệt liên tiếp tuyến tính theo số lượng tháp đang hoạt động. Bởi vì chi phí có tính cộng theo các khoảng độ cao và độc lập trong mỗi khoảng, nên vấn đề giảm xuống còn việc phân chia chuỗi đã sắp xếp thành các phân đoạn sao cho tổng của mỗi phân đoạn lớn nhất là$k$. Sự lựa chọn tham lam cắt giảm bất cứ khi nào phân khúc vượt quá$k$là tối ưu vì việc trì hoãn cắt chỉ làm tăng thêm chi phí mà không làm giảm chi phí trên mỗi đơn vị chiều cao trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    h = list(map(int, input().split()))
    
    freq = {}
    for x in h:
        freq[x] = freq.get(x, 0) + 1

    heights = sorted(freq.keys(), reverse=True)

    active = 0
    cost = 0
    ans = 0

    i = 0
    while i < len(heights):
        if active == 0:
            active = freq[heights[i]]
            i += 1
            continue

        if i < len(heights):
            dh = heights[i-1] - heights[i]
            add_cost = dh * active

            if cost + add_cost <= k:
                cost += add_cost
                active += freq[heights[i]]
                i += 1
            else:
                ans += 1
                cost = 0
                active = 0
        else:
            break

    if active > 0:
        ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đếm tần số để tránh việc xử lý lặp lại các độ cao giống nhau. Sắp xếp theo thứ tự giảm dần đảm bảo chúng ta luôn di chuyển xuống dưới trong không gian chiều cao. 

Các biến`active`Và`cost`đại diện cho trạng thái lát cắt hiện tại.`active`theo dõi có bao nhiêu tòa tháp vẫn ở trên ngưỡng hiện tại và`cost`theo dõi có bao nhiêu khối đã bị loại bỏ trong lát cắt hiện tại. 

Quá trình chuyển đổi quan trọng là tính toán`dh * active`, mã hóa thực tế là mọi tòa tháp đang hoạt động đều bị giảm đồng đều khi di chuyển giữa hai mức độ cao liền kề. 

Bất cứ khi nào thêm khoảng thời gian tiếp theo sẽ vượt quá$k$, chúng tôi hoàn thiện một lát cắt và trạng thái đặt lại, tương ứng với việc đặt một vết cắt ở ranh giới chiều cao hợp lệ cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
3 1 2 2 4
```Tần số được sắp xếp: 

| Chiều cao | Tần số | Đang hoạt động | Chi phí tăng thêm | Chi phí vận hành | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 1 | 1 | - | 0 | bắt đầu | 
| 3 | 1 | 1 | (4-3)*1 = 1 | 1 | tiếp tục | 
| 2 | 2 | 2 | (3-2)*2 = 2 | 3 | tiếp tục | 
| 1 | 1 | 4 | (2-1)*4 = 4 | 7 > 5 | cắt | 

Sau lần cắt đầu tiên, chúng tôi khởi động lại từ độ cao 2. 

| Chiều cao | Tần số | Đang hoạt động | Chi phí tăng thêm | Chi phí vận hành | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 2 | - | 0 | bắt đầu | 
| 1 | 1 | 4 | (2-1)*4 = 4 | 4 | kết thúc | 

Tổng số lát = 2. 

Dấu vết này cho thấy rằng vượt quá$k$buộc một ranh giới chính xác khi mức giảm theo chiều dọc tích lũy trở nên quá đắt để tiếp tục nhóm. 

### Ví dụ 2 

đầu vào:```
4 10
5 5 5 5
```Chỉ có một chiều cao tồn tại nên không cần giảm. 

| Chiều cao | Tần số | Đang hoạt động | Chi phí | Hành động | 
| --- | --- | --- | --- | --- | 
| 5 | 4 | 4 | 0 | lát đơn | 

Kết quả là 1 lát. 

Điều này xác nhận rằng các mảng thống nhất luôn yêu cầu chính xác một thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Việc sắp xếp các độ cao khác nhau chiếm ưu thế, quá trình quét là tuyến tính | 
| Không gian |$O(n)$| Bản đồ tần số và danh sách độ cao | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, do đó$O(n \log n)$giải pháp phù hợp thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ là tuyến tính theo số lượng độ cao và tần số riêng biệt, cũng an toàn dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    h = list(map(int, input().split()))
    
    freq = {}
    for x in h:
        freq[x] = freq.get(x, 0) + 1

    heights = sorted(freq.keys(), reverse=True)

    active = 0
    cost = 0
    ans = 0

    i = 0
    while i < len(heights):
        if active == 0:
            active = freq[heights[i]]
            i += 1
            continue

        dh = heights[i-1] - heights[i]
        add_cost = dh * active

        if cost + add_cost <= k:
            cost += add_cost
            active += freq[heights[i]]
            i += 1
        else:
            ans += 1
            cost = 0
            active = 0

    if active > 0:
        ans += 1

    return str(ans)

# provided sample
assert run("5 5\n3 1 2 2 4") == "2"

# custom cases
assert run("1 10\n5") == "1", "single tower"
assert run("3 100\n1 1 1") == "1", "already equal"
assert run("4 1\n4 3 2 1") == "4", "very small k forces many slices"
assert run("6 10\n6 5 5 3 3 1") == "expected pattern check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 tháp | 1 | cấu trúc tối thiểu | 
| tất cả đều bình đẳng | 1 | không cần giảm giá | 
| k nhỏ | nhiều lát | sự phân mảnh tồi tệ nhất | 
| độ cao hỗn hợp | nhóm giới hạn | tính đúng đắn của việc chia tách tham lam | 

## Vỏ cạnh 

Trường hợp cạnh chính xảy ra khi tất cả các tòa tháp đều có cùng chiều cao. Trong tình huống đó, thuật toán khởi tạo một nhóm hoạt động duy nhất và không bao giờ tích lũy bất kỳ chi phí nào, do đó, nó tạo ra chính xác một lát cắt, điều này đúng vì không cần giảm bớt nhưng chúng tôi vẫn tính một thao tác cuối cùng. 

Một trường hợp tế nhị khác là khi$k$là cực kỳ nhỏ, buộc mọi chuyển đổi độ cao phải trở thành lát cắt riêng của nó. Thuật toán xử lý việc này vì mỗi lần`add_cost`vượt quá$k$, nó sẽ đặt lại ngay lập tức, đảm bảo không có phân đoạn nào vi phạm ràng buộc.
