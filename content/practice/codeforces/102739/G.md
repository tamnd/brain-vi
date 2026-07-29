---
title: "CF 102739G - \u041f\u043b\u0430\u043d \u0414"
description: "Chúng ta có n nhiệm vụ con độc lập phải được hoàn thành trong m ngày liên tiếp. Mỗi công việc con có một khoảng thời gian cho phép: có thể thực hiện vào bất kỳ ngày nào từ si đến fi."
date: "2026-07-29T01:09:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102739
codeforces_index: "G"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2020.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102739
solve_time_s: 61
verified: true
draft: false
---

[CF 102739G - \u041f\u043b\u0430\u043d \u0414](https://codeforces.com/problemset/problem/102739/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`các nhiệm vụ phụ độc lập phải được hoàn thành trong quá trình`m`ngày liên tiếp. Mỗi nhiệm vụ con có một khoảng thời gian cho phép: nó có thể được thực hiện vào bất kỳ ngày nào từ`s_i`ĐẾN`f_i`. Một nhiệm vụ phụ chiếm chính xác một đơn vị ngày, nghĩa là một số nhiệm vụ phụ có thể được thực hiện trong cùng một ngày, nhưng chúng tôi muốn con số hàng ngày đó càng nhỏ càng tốt. 

Mục tiêu là tìm ra một lịch trình trong đó mọi nhiệm vụ phụ được giao cho một ngày hợp lệ và số lượng nhiệm vụ phụ tối đa được giao cho bất kỳ ngày nào được giảm thiểu. Đầu ra là tải tối đa tối thiểu có thể có này, theo sau là một phân phối nhiệm vụ hợp lệ giữa các ngày. 

Giới hạn cho phép lên tới`300000`nhiệm vụ phụ và`300000`ngày. Một cách tiếp cận kiểm tra nhiều phép gán có thể là không thể vì không gian tìm kiếm tăng theo cấp số nhân. Ngay cả một mô phỏng thử mọi nhiệm vụ con vào mỗi ngày có thể cũng sẽ yêu cầu khoảng`n * m`, đó là về`9 * 10^10`hoạt động trong trường hợp lớn nhất. Chúng ta cần một giải pháp gần`O((n + m) log n)`hoặc`O((n + m) log^2 n)`. 

Khó khăn chính không chỉ là quyết định liệu có thể đạt được mức tải tối đa hàng ngày nhất định hay không mà còn là việc xây dựng lịch trình thực tế. Một giải pháp đúng phải xử lý các khoảng thời gian chồng chéo nhiều, những ngày không có nhiệm vụ nào và các nhiệm vụ chỉ có một ngày khả thi. 

Hãy xem xét một nhiệm vụ duy nhất:```
1 1
```với một ngày có sẵn. Câu trả lời là`1`, bởi vì nhiệm vụ duy nhất phải được thực hiện vào ngày đầu tiên. Một giải pháp khởi tạo câu trả lời từ tải trung bình`ceil(n / m)`nhưng quên mất những khoảng thời gian tập trung không thể thực hiện được có thể thất bại trong những trường hợp như vậy. 

Một ví dụ khác là:```
3 3
1 1
1 1
1 3
```Câu trả lời đúng là`2`. Cả hai nhiệm vụ đầu tiên đều phải được thực hiện vào ngày đầu tiên, vì vậy không thể tải tối đa một nhiệm vụ. Một phương pháp tham lam bất cẩn chỉ nhìn vào tổng số nhiệm vụ mỗi ngày mà không xem xét thời hạn có thể cho rằng một nhiệm vụ mỗi ngày là đủ một cách sai lầm. 

Trường hợp cạnh cuối cùng là:```
2 5
2 2
5 5
```Câu trả lời là`1`. Ngày thứ ba và thứ tư không có việc gì, nhưng điều đó không thành vấn đề. Mỗi nhiệm vụ chỉ cần một ngày hợp lệ. Việc triển khai vô tình yêu cầu nhận nhiệm vụ hàng ngày sẽ từ chối lịch trình hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là cố gắng xây dựng lịch trình trong khi đoán số lượng nhiệm vụ phụ tối đa được phép mỗi ngày. 

Giả sử chúng ta biết câu trả lời nhiều nhất là`x`. Chúng ta có thể kiểm tra xem điều này có khả thi hay không bằng cách quét qua các ngày từ trái sang phải. Mọi nhiệm vụ con có ngày bắt đầu đã đến đều có sẵn. Trong số tất cả các nhiệm vụ phụ có sẵn, chúng ta nên luôn chọn những nhiệm vụ có ngày hoàn thành sớm nhất. Đây là quy tắc tham lam về thời hạn sớm nhất theo tiêu chuẩn. Nếu một nhiệm vụ sắp hết hạn, việc trì hoãn nó sẽ rất nguy hiểm vì các nhiệm vụ sau này có thể vẫn linh hoạt hơn. 

Đối với một cố định`x`, việc kiểm tra tham lam này là đúng vì bất cứ khi nào có lịch trình, việc chuyển một nhiệm vụ đã chọn có thời hạn nhỏ nhất sang ngày hiện tại không thể khiến những ngày trong tương lai trở nên khó khăn hơn. Chúng ta đang sử dụng những nhiệm vụ cấp bách nhất trước tiên, vì vậy không có nhiệm vụ nào bị mất một cách không cần thiết. 

Tuy nhiên, việc kiểm tra một giá trị của`x`là không đủ. Chúng ta cần giá trị hợp lệ nhỏ nhất. Câu trả lời là đơn điệu: nếu công suất`x`hoạt động thì bất kỳ công suất lớn hơn nào cũng hoạt động. Điều này cho phép tìm kiếm nhị phân trên câu trả lời. 

Phiên bản bạo lực sẽ thử mọi khả năng có thể và xây dựng lại lịch trình nhiều lần. Đang thử mọi khả năng lên tới`n`sẽ quá chậm vì mỗi lần kiểm tra tính khả thi đều tốn chi phí`O((n + m) log n)`, đưa ra về`O(n(n + m) log n)`hoạt động. 

Quan sát quan trọng là vị từ khả thi là đơn điệu. Tìm kiếm nhị phân làm giảm số lần kiểm tra xuống còn khoảng`log n`và quét tham lam sẽ xây dựng lịch trình trong lần kiểm tra thành công cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n(n + m) log n) | O(n + m) | Quá chậm | 
| Tối ưu | O((n + m) log n log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi nhiệm vụ phụ vào ngày bắt đầu của nó. Trong quá trình quét, điều này cho phép chúng tôi nhanh chóng khám phá những nhiệm vụ phụ nào có sẵn vào ngày hiện tại. 
2. Tìm kiếm nhị phân dung lượng hàng ngày nhỏ nhất có thể`x`. Giới hạn dưới là`1`, và giới hạn trên là`n`, bởi vì thực hiện mọi nhiệm vụ phụ trong một ngày luôn là giới hạn trên nếu khoảng thời gian cho phép. 
3. Đối với từng năng lực ứng viên`x`, thực hiện quét từ trái sang phải trong tất cả các ngày. Thêm tất cả các nhiệm vụ phụ với`s_i`bằng ngày hiện tại thành một đống tối thiểu được sắp xếp bởi`f_i`. 
4. Trong khi ngày hiện tại vẫn còn dung lượng chưa được sử dụng, hãy liên tục loại bỏ nhiệm vụ có ngày hoàn thành nhỏ nhất khỏi heap và gán nó cho ngày hiện tại. 
5. Nếu heap chứa một nhiệm vụ có ngày kết thúc nhỏ hơn ngày hiện tại thì khả năng ứng viên là không thể. Nhiệm vụ đó đã bị bỏ lỡ mỗi ngày có thể. 
6. Sau khi tất cả các ngày được xử lý, hãy kiểm tra xem mọi nhiệm vụ đã được giao chưa. Nếu có, năng lực của ứng viên sẽ hoạt động và chúng tôi tiếp tục tìm kiếm câu trả lời nhỏ hơn. Nếu không, chúng tôi sẽ tăng công suất. 
7. Sau khi tìm kiếm nhị phân tìm thấy dung lượng tối thiểu, hãy chạy cấu trúc tham lam lần cuối với dung lượng đó và in phép gán kết quả. 

Tại sao nó hoạt động: 

Điều bất biến trong quá trình quét tham lam là mọi tác vụ hiện có trong heap đều đang chờ một trong những ngày hiện tại hoặc tương lai. Việc chọn thời hạn nhỏ nhất trước tiên sẽ giúp những nhiệm vụ hạn chế nhất không bị trì hoãn. Nếu quá trình tham lam này thất bại, điều đó có nghĩa là một số nhiệm vụ không còn vị trí hợp lệ nào, do đó không thể tồn tại lịch trình với năng lực đã kiểm tra. Nếu thành công, mọi nhiệm vụ đã được giao trong khi vẫn tôn trọng cả khoảng thời gian và giới hạn hàng ngày của nó. Tìm kiếm nhị phân sau đó tìm ra dung lượng nhỏ nhất mà bất biến này có thể được duy trì. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

n, m = map(int, input().split())
by_start = [[] for _ in range(m + 2)]

for i in range(1, n + 1):
    s, f = map(int, input().split())
    by_start[s].append((f, i))

def check(cap, need_plan=False):
    heap = []
    ans = [[] for _ in range(m + 1)]
    done = 0

    for day in range(1, m + 1):
        for item in by_start[day]:
            heapq.heappush(heap, item)

        if heap and heap[0][0] < day:
            return None

        take = min(cap, len(heap))
        for _ in range(take):
            f, idx = heapq.heappop(heap)
            if need_plan:
                ans[day].append(idx)
            done += 1

        if heap and heap[0][0] < day:
            return None

    if done != n:
        return None

    if need_plan:
        return ans
    return True

lo, hi = 1, n
while lo < hi:
    mid = (lo + hi) // 2
    if check(mid):
        hi = mid
    else:
        lo = mid + 1

plan = check(lo, True)

out = [str(lo)]
for day in range(1, m + 1):
    out.append(str(len(plan[day])) + (" " + " ".join(map(str, plan[day])) if plan[day] else ""))

print("\n".join(out))
```Mảng`by_start`nhóm các nhiệm vụ vào ngày đầu tiên khi chúng xuất hiện. Điều này tránh việc quét tất cả các nhiệm vụ mỗi ngày. 

Heap lưu trữ các nhiệm vụ có sẵn được sắp xếp theo ngày hoàn thành. Python so sánh các bộ dữ liệu theo từ điển, vì vậy`(finish, index)`tự động đưa ra thời hạn sớm nhất trước tiên đồng thời vẫn giữ số lượng nhiệm vụ có sẵn cho đầu ra. 

chức năng`check`được sử dụng cho cả tìm kiếm nhị phân và tạo ra câu trả lời cuối cùng. Trong quá trình tìm kiếm nhị phân, nó chỉ cần biết liệu dung lượng có hoạt động hay không, trong khi lệnh gọi cuối cùng ghi lại số tác vụ đã chọn. 

Việc kiểm tra thời hạn trước và sau khi thực hiện nhiệm vụ sẽ ngăn chặn nhiệm vụ bị bỏ lỡ tồn tại sang ngày sau đó. Lần kiểm tra thứ hai rất hữu ích vì sau khi phân công nhiệm vụ của ngày hiện tại, thời hạn nhỏ nhất tiếp theo có thể vẫn không hợp lệ. 

Không cần số học số nguyên lớn vì tất cả các bộ đếm đều có nhiều nhất`300000`. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào này:```
3 3
1 1
1 1
1 3
```Tìm kiếm nhị phân cuối cùng kiểm tra khả năng`2`. 

| Ngày | Nhiệm vụ có sẵn | Đống sau khi thêm | Được giao hôm nay | 
| --- | --- | --- | --- | 
| 1 | 1, 2, 3 | 1, 1, 3 | 1, 2 | 
| 2 | không | 3 | 3 | 
| 3 | không | trống | không | 

Hai nhiệm vụ đầu tiên buộc phải có ít nhất hai nhiệm vụ. Sự lựa chọn tham lam sẽ xử lý chúng ngay lập tức vì thời hạn của chúng là nhỏ nhất. 

Cho một ví dụ khác:```
2 5
2 2
5 5
```công suất`1`được thử nghiệm. 

| Ngày | Nhiệm vụ có sẵn | Đống | Được giao | 
| --- | --- | --- | --- | 
| 1 | không | trống | không | 
| 2 | nhiệm vụ 1 | nhiệm vụ 1 | nhiệm vụ 1 | 
| 3 | không | trống | không | 
| 4 | không | trống | không | 
| 5 | nhiệm vụ 2 | nhiệm vụ 2 | nhiệm vụ 2 | 

Dấu vết cho thấy những ngày trống được phép. Yêu cầu duy nhất là mọi nhiệm vụ đều nhận được một ngày hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n log n) | Mỗi lần lặp tìm kiếm nhị phân thực hiện một lần quét vùng heap và có các lần lặp O(log n). | 
| Không gian | O(n + m) | Các nhóm nhiệm vụ, đống và lịch trình cuối cùng đều lưu trữ hầu hết thông tin tuyến tính. | 

Kích thước đầu vào tối đa là`300000`, do đó thuật toán chỉ thực hiện số lần quét đống tuyến tính theo logarit. Điều này phù hợp thoải mái trong giới hạn dự định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data):
    old = sys.stdin
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    by_start = [[] for _ in range(m + 2)]

    for i in range(1, n + 1):
        s, f = map(int, input().split())
        by_start[s].append((f, i))

    import heapq

    def check(cap, plan=False):
        heap = []
        ans = [[] for _ in range(m + 1)]
        cnt = 0

        for day in range(1, m + 1):
            for x in by_start[day]:
                heapq.heappush(heap, x)

            if heap and heap[0][0] < day:
                return None

            for _ in range(min(cap, len(heap))):
                f, idx = heapq.heappop(heap)
                if plan:
                    ans[day].append(idx)
                cnt += 1

        if cnt != n:
            return None
        return ans if plan else True

    lo, hi = 1, n
    while lo < hi:
        mid = (lo + hi) // 2
        if check(mid):
            hi = mid
        else:
            lo = mid + 1

    res = check(lo, True)
    out = [str(lo)]
    for i in range(1, m + 1):
        out.append(str(len(res[i])) + (" " + " ".join(map(str, res[i])) if res[i] else ""))

    sys.stdin = old
    return "\n".join(out)

assert solve("""3 3
1 1
1 1
1 3
""").splitlines()[0] == "2"

assert solve("""2 5
2 2
5 5
""").splitlines()[0] == "1"

assert solve("""1 1
1 1
""").splitlines()[0] == "1"

assert solve("""5 5
1 5
1 5
1 5
1 5
1 5
""").splitlines()[0] == "1"

assert solve("""4 4
1 2
1 2
3 4
3 4
""").splitlines()[0] == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba nhiệm vụ với hai nhiệm vụ cố định trong một ngày | 2 | Buộc chồng chéo và xử lý thời hạn | 
| Hai nhiệm vụ một ngày riêng biệt | 1 | Những ngày trống trải và những khoảng thời gian thưa thớt | 
| Một nhiệm vụ trong một ngày | 1 | Vỏ kích thước tối thiểu | 
| Năm nhiệm vụ toàn diện giống hệt nhau | 1 | Khoảng thời gian linh hoạt | 
| Hai nhóm khoảng cách nhau | 1 | Xử lý đúng các phạm vi độc lập | 

## Vỏ cạnh 

Dành cho:```
1 1
1 1
```heap nhận nhiệm vụ duy nhất vào ngày đầu tiên và giao nó ngay lập tức. Tìm kiếm nhị phân không thể giảm dung lượng xuống dưới một, vì vậy câu trả lời là đúng. 

Vì:```
3 3
1 1
1 1
1 3
```kiểm tra năng lực một lần không thành công. Vào ngày đầu tiên, thuật toán tham lam phải đặt một trong hai nhiệm vụ kết thúc vào ngày đầu tiên, nhưng vẫn còn một nhiệm vụ khác có cùng thời hạn. Khi ngày thứ hai bắt đầu, nhiệm vụ còn lại đó đã hết hạn. Năng lực hai thành công vì cả hai nhiệm vụ cấp bách đều được xử lý ngay lập tức. 

Vì:```
2 5
2 2
5 5
```thuật toán để trống ngày một, ba và bốn. Vùng heap chỉ chứa các nhiệm vụ vào những ngày được phép, do đó không có nhiệm vụ không hợp lệ nào được thực hiện. 

Đối với các khoảng thời gian bao gồm mỗi ngày, chẳng hạn như:```
5 5
1 5
1 5
1 5
1 5
1 5
```thuật toán tham lam có thể phân bổ nhiệm vụ trong nhiều ngày khác nhau. Điều này chứng tỏ tại sao câu trả lời lại phụ thuộc vào những hạn chế về khoảng thời gian thay vì chỉ đơn giản là số lượng nhiệm vụ. 

Tôi cũng có thể điều chỉnh bài xã luận này thành một phiên bản ngắn hơn theo phong cách Codeforces, tập trung vào ý tưởng cốt lõi và bằng chứng nếu bạn muốn.
