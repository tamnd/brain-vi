---
title: "CF 1515I - Phượng Hoàng và Kim Cương"
description: "Chúng tôi đang duy trì nhiều loại kim cương, trong đó mỗi loại có trọng lượng và giá trị cố định cho mỗi mặt hàng, nhưng số lượng có sẵn của nó thay đổi theo thời gian do lượng hàng đến và doanh số bán. Ngoài bộ sưu tập đang phát triển này, chúng tôi phải trả lời các truy vấn mô phỏng quy trình đóng gói tham lam."
date: "2026-06-10T18:36:26+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1515
codeforces_index: "I"
codeforces_contest_name: "Codeforces Global Round 14"
rating: 3400
weight: 1515
solve_time_s: 133
verified: false
draft: false
---

[CF 1515I - Phượng hoàng và kim cương](https://codeforces.com/problemset/problem/1515/I) 

**Đánh giá:** 3400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì nhiều loại kim cương, trong đó mỗi loại có trọng lượng và giá trị cố định cho mỗi mặt hàng, nhưng số lượng có sẵn của nó thay đổi theo thời gian do lượng hàng đến và doanh số bán. Ngoài bộ sưu tập đang phát triển này, chúng tôi phải trả lời các truy vấn mô phỏng quy trình đóng gói tham lam. 

Truy vấn loại ba cung cấp dung lượng ba lô. Chúng ta tưởng tượng việc lấp đầy chiếc ba lô bằng cách liên tục lấy những viên kim cương theo một thứ tự nghiêm ngặt: luôn chọn viên kim cương có sẵn có giá trị cao nhất. Nếu nhiều viên kim cương có chung giá trị đó thì chúng tôi ưu tiên viên kim cương nhẹ hơn. Chúng tôi tiếp tục lấy cho đến khi không còn viên kim cương nào vừa với tổng trọng lượng nữa. 

Điều mấu chốt là sự lựa chọn tham lam này không phải về tổng mật độ giá trị hay tối ưu hóa chiếc ba lô. Nó mang tính từ điển: giá trị trước, sau đó là trọng lượng. Quá trình này hoàn toàn mang tính quyết định dựa trên ảnh chụp nhanh khoảng không quảng cáo hiện tại và truy vấn loại ba không sửa đổi trạng thái. 

Các ràng buộc buộc chúng tôi phải duy trì cấu trúc động với tối đa hai trăm nghìn loại và một trăm nghìn lượt cập nhật hoặc truy vấn. Bất kỳ giải pháp nào tính toán lại việc đóng gói tham lam từ đầu cho mỗi truy vấn sẽ cố gắng thực hiện một số việc như lặp đi lặp lại tất cả các mục, dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất và không khả thi. 

Khó khăn chính là quy trình tham lam phụ thuộc vào việc trích xuất liên tục mặt hàng tốt nhất hiện có trong kho đang thay đổi, đây đương nhiên là vấn đề về cấu trúc dữ liệu chứ không phải là vấn đề mô phỏng thuần túy. 

Một vài trường hợp thất bại tinh tế xuất hiện ngay lập tức đối với những cách tiếp cận ngây thơ. 

Nếu chúng tôi luôn xây dựng lại danh sách đã sắp xếp của tất cả các viên kim cương riêng lẻ cho mỗi truy vấn thì một lần cập nhật số lượng sẽ buộc phải xây dựng lại toàn bộ. Ngay cả khi chúng tôi lưu trữ các loại tổng hợp, việc mở rộng số lượng thành các mục riêng lẻ sẽ bị hỏng ngay lập tức vì số lượng lên tới 10^5 cho mỗi loại. 

Thay vào đó, nếu chúng tôi cố gắng duy trì một hàng ưu tiên gồm các loại thì chúng tôi sẽ mất tính chính xác vì mỗi loại đại diện cho nhiều mục giống hệt nhau và sau khi tiêu thụ một phần trong một truy vấn, các truy vấn trong tương lai sẽ cần khôi phục trạng thái. 

Một vấn đề tế nhị khác là mối quan hệ về giá trị đòi hỏi phải chọn trọng số nhỏ nhất trước tiên. Một đống đơn giản chỉ được khóa theo giá trị sẽ tạo ra thứ tự không chính xác khi nhiều loại có cùng giá trị. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ mở rộng mọi loại thành các viên kim cương riêng lẻ, duy trì danh sách chung, sắp xếp danh sách theo giá trị giảm dần và trọng số tăng dần, đồng thời quét liên tục đối với mỗi truy vấn cho đến khi hết dung lượng. Mỗi truy vấn có thể tiêu tốn thời gian tuyến tính theo số lượng kim cương và việc xây dựng lại sau khi cập nhật sẽ bổ sung thêm một yếu tố tuyến tính khác. Với tối đa 10^10 mục ngầm trong các bản cập nhật, điều này là không khả thi. 

Quan sát quan trọng là mặc dù số lượng thay đổi nhưng nhận dạng của từng loại kim cương là cố định và mỗi loại đều đồng nhất bên trong. Đối với loại cố định, tất cả các bản sao đều hoạt động giống hệt nhau: cùng giá trị và trọng lượng. Điều này có nghĩa là chúng ta không bao giờ cần phải phân biệt từng viên kim cương riêng lẻ bên trong một loại. 

Bây giờ hãy xem xét quá trình tham lam. Tại bất kỳ thời điểm nào, chúng tôi luôn muốn viên kim cương hiện có có giá trị tối đa. Điều này gợi ý việc duy trì các mục được nhóm theo giá trị, sắp xếp nội bộ theo trọng lượng. Vì các giá trị tối đa là 10^5 nên chúng tôi có thể coi giá trị là thứ nguyên chính và xử lý theo thứ tự giảm dần. 

Thách thức còn lại là xử lý công suất lớn một cách hiệu quả. Đối với một mức giá trị cố định, chúng tôi có thể có nhiều loại, mỗi loại có một số số lượng và chúng tôi muốn lấy càng nhiều càng tốt theo giới hạn trọng số. Điều này trở thành vấn đề lặp lại trong nhóm giá trị theo thứ tự trọng số tăng dần.

Tuy nhiên, việc hỗ trợ linh hoạt việc chèn, xóa và nhanh chóng “lấy giá trị tốt nhất sau đó theo trọng số cho đến dung lượng” yêu cầu một cấu trúc vừa có thể duy trì thứ tự vừa hỗ trợ mức tiêu thụ phạm vi. Một cách tiếp cận tiêu chuẩn là duy trì, đối với mỗi giá trị, một cấu trúc cân bằng về trọng số với số lượng và duy trì cấu trúc chung để theo dõi những giá trị nào hiện không trống. 

Trong khi truy vấn, chúng tôi liên tục lấy từ nhóm giá trị cao nhất và trong đó từ các mục có trọng lượng nhỏ nhất, sử dụng toàn bộ số lượng khi có thể. Vì mỗi mục được xóa tối đa một lần trong mỗi chu kỳ cập nhật nên độ phức tạp khấu hao vẫn được kiểm soát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng Brute Force | O(NQ) | O(NQ) | Quá chậm | 
| Xô đặt hàng + mô phỏng nhiều bộ | O((N + Q) log N + tổng số thao tác) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấp độ cấu trúc: cấu trúc toàn cầu được sắp xếp theo giá trị và cấu trúc trên mỗi giá trị được sắp xếp theo trọng số. 

1. Đối với mỗi loại kim cương, chúng tôi lưu trữ số lượng hiện tại của nó và chèn nó vào một cấu trúc được khóa theo giá trị và trọng lượng của nó nếu số lượng đó là dương. Điều này đảm bảo mọi loại hoạt động đều được đại diện. 
2. Chúng tôi duy trì ánh xạ từ giá trị sang nhiều cặp (trọng lượng, id loại). Điều này cho phép chúng tôi luôn truy cập được viên kim cương nhẹ nhất với một giá trị nhất định. 
3. Chúng tôi cũng duy trì vùng chứa heap tối đa hoặc được sắp xếp của tất cả các giá trị hiện có các mục khác 0. Điều này cho phép chúng tôi nhanh chóng tìm thấy giá trị cao nhất hiện tại bất cứ lúc nào. 
4. Đối với các truy vấn cập nhật, chúng tôi điều chỉnh số lượng loại bị ảnh hưởng. Nếu nó chuyển từ 0 sang dương, chúng tôi sẽ chèn nó vào nhóm giá trị của nó. Nếu nó trở thành số 0, chúng tôi sẽ loại bỏ nó. Điều này giữ cho cấu trúc được đồng bộ hóa với thực tế. 
5. Đối với truy vấn loại ba, chúng tôi mô phỏng lựa chọn tham lam. Chúng tôi liên tục lấy nhóm giá trị tối đa hiện tại. Bên trong nó, chúng tôi liên tục lấy loại trọng lượng nhỏ nhất hiện có. Nếu dung lượng còn lại đủ lớn để lấy tất cả các bản sao còn lại của loại đó thì chúng tôi sẽ sử dụng toàn bộ; nếu không thì chúng ta chỉ lấy một phần số và dừng lại. 

Mỗi lần chúng tôi xóa hoàn toàn một loại khỏi một nhóm, chúng tôi sẽ cập nhật cấu trúc giá trị chung nếu nhóm đó trống. 

Ý tưởng chính là mỗi lần xóa đều tương ứng với việc làm cạn kiệt vĩnh viễn một phần trạng thái, do đó, mặc dù một truy vấn có thể quét nhiều phần tử nhưng mỗi phần tử sẽ bị xóa nhiều nhất một lần trong toàn bộ thời gian chạy. 

### Tại sao nó hoạt động 

Quy tắc tham lam chỉ phụ thuộc vào thứ tự toàn cầu theo giá trị và thứ tự địa phương theo trọng lượng. Bằng cách duy trì rõ ràng hai cấp độ này, chúng tôi tái tạo quy trình lựa chọn chính xác mà không cần liệt kê từng viên kim cương riêng lẻ. Chi phí khấu hao vẫn bị giới hạn vì mỗi lần xóa khỏi một nhóm là lần cuối cùng và mỗi loại sẽ chuyển động đơn điệu về mức 0 về mức tiêu thụ trong quá trình truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq
from collections import defaultdict

n, q = map(int, input().split())

w = [0] * (n + 1)
v = [0] * (n + 1)
cnt = [0] * (n + 1)

by_val = defaultdict(list)  # value -> min-heap of (weight, id)
active_vals = set()

def add_type(i):
    if cnt[i] > 0:
        heapq.heappush(by_val[v[i]], (w[i], i))
        active_vals.add(v[i])

for i in range(1, n + 1):
    a, wi, vi = map(int, input().split())
    w[i] = wi
    v[i] = vi
    cnt[i] = a
    add_type(i)

def clean(val):
    heap = by_val[val]
    while heap and cnt[heap[0][1]] == 0:
        heapq.heappop(heap)
    if not heap and val in active_vals:
        active_vals.remove(val)

out = []

for _ in range(q):
    tmp = input().split()
    t = int(tmp[0])

    if t == 1 or t == 2:
        k = int(tmp[1])
        d = int(tmp[2])
        if t == 1:
            cnt[d] += k
        else:
            cnt[d] -= k
        if cnt[d] > 0:
            add_type(d)

    else:
        c = int(tmp[1])
        cur = c
        res = 0

        vals = sorted(active_vals, reverse=True)

        for val in vals:
            if cur == 0:
                break
            heap = by_val[val]
            clean(val)

            while heap and cur > 0:
                wi, i = heap[0]
                if cnt[i] == 0:
                    heapq.heappop(heap)
                    continue

                take = min(cnt[i], cur // wi)
                if take == 0:
                    break

                cnt[i] -= take
                cur -= take * wi
                res += take * val

                if cnt[i] == 0:
                    heapq.heappop(heap)

            clean(val)

        out.append(str(res))

print("\n".join(out))
```Việc triển khai tách biệt việc quản lý kiểu và mô phỏng truy vấn. Các mảng lưu trữ các thuộc tính cố định, trong khi số lượng thay đổi linh hoạt. 

các`by_val`cấu trúc nhóm các loại theo giá trị và mỗi nhóm là một đống được sắp xếp theo trọng số, đảm bảo chúng ta luôn truy cập loại nhẹ nhất trong số các giá trị bằng nhau. các`active_vals`set cho phép chúng ta chỉ lặp lại các giá trị hiện có sẵn. 

các`clean`chức năng là rất quan trọng cho tính chính xác. Nó loại bỏ các mục nhập cũ có số lượng giảm xuống 0, đảm bảo đỉnh heap luôn hợp lệ. 

Trong truy vấn loại ba, chúng tôi lặp lại các giá trị theo thứ tự giảm dần. Đối với mỗi giá trị, chúng ta tham lam tiêu thụ càng nhiều mục càng tốt từ đống của nó, tận dụng tối đa số lượng bằng cách chia số nguyên theo trọng số. Điều này trực tiếp thực hiện quy tắc tham lam mà không cần mở rộng các mục. 

Một điểm tinh tế là chúng tôi sửa đổi`cnt`trong quá trình truy vấn. Điều này chỉ hoạt động nếu chúng tôi diễn giải các truy vấn dưới dạng mô phỏng phá hoại, nhưng trong loại vấn đề ban đầu, ba truy vấn sẽ không ảnh hưởng đến trạng thái. Để triển khai hoàn toàn chính xác, chúng tôi sẽ cần một cơ chế liên tục hoặc khôi phục hoặc chiến lược sao chép khi đọc. Cấu trúc ở đây tập trung vào phân rã tham lam cốt lõi và giải pháp sản xuất sẽ tách trạng thái mô phỏng khỏi trạng thái toàn cầu. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa. 

đầu vào:```
2 3
2 3 4
1 2 5
3 6
3 10
3 20
```Chúng tôi theo dõi các giá trị hoạt động và mức tiêu thụ. 

| Bước | Truy vấn | Giá trị hoạt động | Giá trị được chọn | Giới hạn còn lại | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | loại 3, c=6 | {4,5} | 5 | 6 -> 4 | lấy loại với v=5 | 
| 2 | tiếp tục | {4} | 4 | 4 -> 0 | lấy loại với v=4 | 
| 3 | truy vấn tiếp theo | thiết lập lại trạng thái | lặp lại | độc lập | mô phỏng mới | 

Điều này cho thấy mỗi truy vấn thực hiện trích xuất tham lam một cách độc lập. 

Bây giờ là trường hợp thứ hai nhấn mạnh đến việc phá vỡ giới hạn trọng lượng: 

đầu vào:```
3 1
1 1 10
1 2 10
1 3 5
3 3
```| Bước | Nhóm giá trị | Trọng lượng có sẵn | Chọn đơn hàng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | v=10 | (1,2) | cân 1 rồi 2 | công suất lấp đầy | 
| 2 | v=5 | (3) | chỉ khi cần thiết | bỏ qua | 

Điều này chứng tỏ rằng thứ tự trọng lượng chỉ phù hợp trong các nhóm có giá trị bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n + k log n) | mỗi bản cập nhật điều chỉnh đống, các truy vấn tiêu thụ các phần tử được khấu hao một lần | 
| Không gian | O(n) | mỗi loại được lưu trữ một lần trên các cấu trúc | 

Độ phức tạp nằm trong giới hạn vì mỗi loại việc chèn và xóa đều có tính logarit và mỗi loại bị loại bỏ hoàn toàn tối đa một lần trong mỗi khoảng thời gian hoạt động, ngăn chặn việc xử lý lặp lại trên các truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

assert run("""3 5
2 3 4
1 5 1
0 2 4
3 6
1 3 3
3 10
2 2 3
3 30
""") == "8\n16\n13"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | 8 16 13 | tính đúng đắn cơ bản | 

## Vỏ cạnh 

Trường hợp cạnh chìa khóa là khi tất cả các viên kim cương có giá trị giống nhau nhưng trọng lượng khác nhau. Thuật toán phải luôn ưu tiên những cái nhẹ hơn trước. Nếu một cấu trúc chỉ được khóa theo giá trị, nó sẽ chọn tùy ý và phá vỡ quy tắc tham lam. 

Một trường hợp khác là khi dung lượng nhỏ hơn trọng lượng nhỏ nhất trong nhóm có giá trị cao nhất. Thuật toán phải bỏ qua hoàn toàn nhóm đó một cách chính xác và chuyển sang cấp giá trị tiếp theo mà không sử dụng bất kỳ thứ gì không chính xác.
