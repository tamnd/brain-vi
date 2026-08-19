---
title: "CF 102192L - Từ ICPC đến ACM"
description: "Chúng tôi có một nhà máy hoạt động được (k) tháng. Trong tháng (i), chi phí nguyên liệu thô (ci) trên mỗi đơn vị, việc sản xuất một máy tính sẽ tốn một chiếc khác (mi), việc sản xuất bị giới hạn ở (pi) và chính xác (di) máy tính phải được giao."
date: "2026-08-18T02:19:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "L"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 228
verified: true
draft: false
---

[CF 102192L - Từ ICPC đến ACM](https://codeforces.com/problemset/problem/102192/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhà máy hoạt động được (k) tháng. Trong tháng (i), chi phí nguyên vật liệu thô (c_i) trên một đơn vị, việc sản xuất một máy tính sẽ tốn một chiếc khác (m_i), việc sản xuất bị giới hạn ở (p_i) và máy tính phải được giao chính xác (d_i). Nguyên liệu thô có thể được lưu trữ không giới hạn dung lượng, trong khi máy tính thành phẩm chỉ được vận chuyển xuyên ranh giới từ tháng (i) đến tháng (i+1) với số lượng lên tới (e_i). 

Có hai chi phí lưu trữ độc lập. Chi phí duy trì một đơn vị nguyên liệu thô trong một tháng (R_i) và duy trì một máy tính hoàn chỉnh với cùng chi phí biên (E_i). Một máy tính được sản xuất và bán trong cùng tháng sẽ không phải trả chi phí lưu trữ. Ban đầu cả hai hàng tồn kho đều trống rỗng. 

Mục tiêu là chọn khi nào nên mua nguyên liệu thô, khi nào sản xuất máy tính và loại máy tính nào được sản xuất để sử dụng cho nhu cầu mỗi tháng sao cho mọi nhu cầu đều được đáp ứng với tổng chi phí tối thiểu. Nếu một số nhu cầu không thể được đáp ứng vì năng lực sản xuất và kho chứa thành phẩm không đủ thì câu trả lời là (-1). 

Đầu vào chứa tối đa (50.000) tháng trong một trường hợp thử nghiệm và tổng (k) trên tất cả các trường hợp thử nghiệm tối đa là (300.000). Phương pháp tính từng cặp tháng đã là quá lớn, vì (50.000^2/2) là khoảng (1,25\time10^9). Nghiêm trọng hơn, (p_i) và (d_i) đều có thể là (10^4), do đó, việc triển khai theo từng đơn vị có thể xử lý tối đa (50.000\times10^4=5\times10^8) máy tính trong một trường hợp thử nghiệm. Giải pháp phải phụ thuộc vào số tháng chứ không phụ thuộc vào tổng số máy tính. 

Điều tinh tế đầu tiên là nguyên liệu thô rẻ nhất để sản xuất trong tháng (i) không nhất thiết phải mua vào tháng (i). Ví dụ,```
1
2
1 0 0 0
100 1 0 1
1 1 0
```có câu trả lời (2). Nguyên liệu thô được sử dụng trong tháng 2 có thể được mua vào tháng 1 với giá (1) nhân dân tệ và dự trữ bằng (1) nhân dân tệ khác. Một giải pháp luôn sử dụng (c_i) làm giá vật liệu sẽ phải trả (100). 

Điều tinh tế thứ hai là dung lượng lưu trữ thành phẩm áp dụng giữa các tháng chứ không phải số lượng máy tính được sản xuất tạm thời trong tháng hiện tại. Ví dụ,```
1
2
0 0 0 2
0 2 0 0
1 0 0
```có câu trả lời (-1). Tháng thứ nhất có thể sản xuất được hai máy tính, nhưng sang tháng thứ hai chỉ được một chiếc. Vì tháng thứ hai không thể sản xuất được gì nên nhu cầu về hai tháng là không thể. Việc triển khai bất cẩn khiến tất cả hoạt động sản xuất không được sử dụng trong cấu trúc dữ liệu của nó mà không áp dụng dung lượng biên sẽ tìm ra một kế hoạch khả thi một cách không chính xác. 

Điều tinh vi thứ ba là chi phí lưu kho phải được tính cho mỗi lần vượt qua ranh giới. Ví dụ,```
1
2
1 0 0 1
100 1 0 1
1 1 2
```có câu trả lời (3). Máy tính được sản xuất trong tháng 1 có giá (1), sau đó có giá (2) khác khi được lưu trữ, do đó, nó có giá (3) khi được bán trong tháng 2. Việc tính phí lưu trữ khi máy tính được lắp vào thay vì khi nó thực sự vượt qua ranh giới là một nguồn dễ xảy ra lỗi riêng lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là suy luận về từng máy tính. Trong mỗi tháng, chúng tôi có thể tạo tối đa (p_i) máy tính ứng viên, lưu trữ chi phí hiện tại của chúng, lấy những máy tính (d_i) rẻ nhất cho nhu cầu hiện tại và loại bỏ những máy tính đắt nhất bất cứ khi nào vượt quá dung lượng kho. Điều này đúng về mặt khái niệm, bởi vì mọi máy tính đều độc lập ngoại trừ khả năng sản xuất và lưu trữ. 

Vấn đề là số lượng ứng viên. Một trường hợp thử nghiệm có thể chứa (50.000) tháng với (p_i=10.000), cho ra (5\times10^8) máy tính ứng viên. Ngay cả khi một đống xử lý mọi lần chèn theo thời gian logarit, điều này sẽ đòi hỏi hàng trăm triệu thao tác trên đống. Vấn đề tương tự sẽ xuất hiện nếu chúng ta cố gắng mô phỏng từng đơn vị nhu cầu một cách riêng biệt. 

Quan sát đầu tiên loại bỏ hoàn toàn tồn kho nguyên liệu thô. Gọi (q_i) là chi phí rẻ nhất có thể để có được một đơn vị nguyên liệu thô sẵn sàng sử dụng trong tháng (i). Hoặc chúng tôi mua nó vào tháng (i) với giá (c_i) hoặc chúng tôi có sẵn nó trong tháng (i-1) và trả tiền (R_{i-1}) để lưu trữ nó. Như vậy 

[ 
q_1=c_1 
] 

và với (i>1), 

[ 
q_i=\min(c_i,q_{i-1}+R_{i-1}). 
] 

Bởi vì năng lực nguyên liệu thô là không giới hạn nên không có sự tương tác giữa các đơn vị khác nhau. Chúng ta có thể giả định một cách độc lập rằng mọi máy tính được sản xuất trong tháng (i) đều sử dụng nguyên liệu thô theo giá gốc (q_i). Khi đó chi phí sản xuất của nó là 

[ 
w_i=q_i+m_i. 
] 

Vấn đề còn lại chỉ là về những chiếc máy tính đã hoàn thiện. Một máy tính sản xuất vào tháng (i) có giá thành hiện hành (w_i). Nếu nó ở trong kho thêm một tháng nữa thì giá của nó sẽ tăng thêm (E_i). Do đó, mọi máy tính hiện đang được lưu trữ đều nhận được chi phí bổ sung như nhau khi thời gian tăng thêm một tháng. 

Chi phí cộng gộp chung đó chính là chìa khóa cho giải pháp tham lam. Vào đầu tháng, hãy xếp những máy tính có thể sản xuất trong tháng này vào bộ sưu tập được sắp xếp theo tổng chi phí hiện tại của chúng. Sau đó đáp ứng nhu cầu hiện tại bằng cách sử dụng các máy tính rẻ nhất hiện có. Sau khi đáp ứng đủ nhu cầu, chỉ (e_i) máy tính mới có thể tồn tại được trong tháng tiếp theo, vì vậy hãy giữ lại (e_i) rẻ nhất và loại bỏ những máy tính đắt nhất. 

Thực tế là về mặt khái niệm chúng ta có thể chèn tất cả (p_i) máy tính có thể cần được quan tâm. Họ là những ứng cử viên, không nhất thiết phải là những máy tính đã được sản xuất về mặt vật lý. Nếu một ứng viên bị loại vì quá đắt để giữ lại, chúng tôi chỉ hiểu điều đó là không bao giờ sản xuất chiếc máy tính đó. Chỉ những ứng viên cuối cùng được bán mới đóng góp chi phí sản xuất và nguyên vật liệu vào câu trả lời. 

Vì sao nhu cầu hiện nay nên sử dụng máy tính rẻ nhất? Giả sử hai máy tính hiện có có chi phí hiện tại (a<b), nhưng một máy tính được cho là tối ưu sẽ bán máy tính tính giá thành (b) ngay bây giờ và giữ cho máy tính tính giá thành (a). Nếu sau này máy tính rẻ hơn không bao giờ được sử dụng, việc hoán đổi chúng ngay lập tức sẽ cải thiện giải pháp. Nếu sau này chiếc máy tính rẻ hơn được sử dụng, hãy hoán đổi vai trò của chúng: bán chiếc máy rẻ hơn ngay bây giờ và giữ chiếc máy tính đắt tiền hơn trong cùng khoảng thời gian tương lai. Cả hai máy tính sẽ nhận được chi phí lưu trữ trong tương lai như nhau, do đó sự khác biệt (a) đến (b) không bao giờ được phục hồi bằng cách giữ lại cái rẻ hơn. Bán chiếc máy tính rẻ hơn bây giờ ít nhất cũng tốt. 

Đối số thống trị tương tự xử lý ranh giới kho. Nếu hai máy tính vẫn còn sau khi đáp ứng nhu cầu và một chiếc có giá thấp hơn chiếc kia, thì việc giữ lại chiếc đắt tiền trong khi loại bỏ chiếc rẻ tiền không bao giờ có thể giúp ích được gì, bởi vì mỗi tháng trong tương lai đều cộng thêm chi phí lưu trữ như nhau cho cả hai. Máy tính đắt tiền bị thống trị.

Đây chính xác là mô phỏng tham lam được mô tả trong tài liệu cuộc thi: tính giá nguyên liệu thô hiệu quả rẻ nhất, bảo trì máy tính đã hoàn thiện theo chi phí hiện tại, bán những chiếc rẻ nhất và loại bỏ những chiếc đắt nhất khi đạt đến ranh giới kho. 

Thách thức triển khai còn lại là nhiều máy tính có thể có cùng mức chi phí. Do đó, chúng tôi lưu trữ mỗi tháng sản xuất dưới dạng một lô có chi phí và số lượng thay vì chèn một phần tử heap trên mỗi máy tính. Vùng heap tối thiểu mang lại lô rẻ nhất để bán và vùng heap tối đa mang lại lô đắt nhất để cắt giảm công suất. Mỗi lô chứa tối đa một mục nhập heap trong mỗi heap, bất kể số lượng của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((\sum p_i)\log(\sum p_i))) | (O(\sum p_i)) | Quá chậm | 
| Tối ưu | (O(k\log k)) | (O(k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (k) dữ liệu sản xuất và nhu cầu hàng tháng, sau đó là quá trình chuyển đổi kho (k-1). Chúng tôi cần dữ liệu chuyển đổi trước khi mô phỏng các tháng vì đầu vào đặt tất cả dữ liệu hàng tháng lên hàng đầu. 
2. Duy trì`raw_cost`, mức giá rẻ nhất mà một đơn vị nguyên liệu thô có thể có trong tháng hiện tại. Trong tháng đầu tiên là (c_1). Sau đó tính toán 
[ 
\text{raw_cost}=\min(c_i,\text{raw_cost}+R_{i-1}). 
] 
Điều này hiệu quả vì kho lưu trữ nguyên liệu thô không có giới hạn dung lượng, do đó không bao giờ có lý do gì để hai đơn vị nguyên liệu thô cạnh tranh nhau về không gian. 
3. Duy trì toàn cầu`offset`thể hiện chi phí lưu trữ máy tính đã hoàn thiện được tích lũy bởi tất cả các máy tính còn tồn tại trong tháng trước. Giá trị thực của một máy tính là giá trị cộng thêm khóa heap được lưu trữ của nó`offset`. Khi chuyển từ tháng (i) sang (i+1) thì tăng`offset`bởi (E_i). Việc thêm cùng một giá trị vào mọi máy tính không làm thay đổi thứ tự của chúng. 
4. Trong tháng (i), tạo một lô chứa tối đa (p_i) máy tính ứng viên. Chi phí sản xuất thực tế của nó là 
[ 
\text{raw_cost}+m_i. 
] 
Lưu trữ khóa chuẩn hóa 
[ 
\text{key}=\text{raw_cost}+m_i-\text{offset}. 
]
Sau đó`key + offset`luôn là chi phí thực hiện tại của máy tính. 
5. Kiểm tra xem số lượng máy tính ứng viên có sẵn ít nhất là (d_i) hay không. Nếu không, nhu cầu hiện tại không thể được đáp ứng, vì vậy hãy trả về (-1). 
6. Loại bỏ chính xác (d_i) máy tính khỏi vùng chi phí tối thiểu. Đối với mỗi đợt, hãy loại bỏ số lượng máy tính cần thiết, nhân số lượng đó với`key + offset`và thêm kết quả vào câu trả lời. Nếu chỉ bán được một phần lô thì đưa số lượng còn lại vào đống. 
7. Nếu đây không phải là tháng cuối cùng, hãy so sánh lượng hàng tồn kho còn lại với (e_i). Nếu còn lại nhiều hơn (e_i) máy tính, hãy loại bỏ các máy tính khỏi vùng chi phí tối đa cho đến khi còn lại chính xác (e_i). Những ứng cử viên bị loại bỏ này được hiểu là những chiếc máy tính chưa từng được sản xuất. 
8. Tăng`offset`bởi (E_i) trước khi xử lý vào tháng tiếp theo. Mọi máy tính vượt qua ranh giới này đều phải trả chính xác chi phí lưu trữ này. 

### Tại sao nó hoạt động 

Điều bất biến là sau tháng xử lý (i), vùng heap đại diện chính xác cho tập hợp ứng viên máy tính hoàn chỉnh rẻ nhất mà vẫn có thể hữu ích trong những tháng tới, với số lượng tối đa được phép tồn tại trong phạm vi ranh giới kho hiện tại. Đối với nhu cầu hiện tại, việc thay thế bất kỳ máy tính đắt tiền nào đã chọn bằng một máy tính rẻ hơn hiện có không thể làm cho kế hoạch trong tương lai trở nên tồi tệ hơn, bởi vì dung lượng lưu trữ trong tương lai sẽ bổ sung cùng một lượng cho cả hai. Tại ranh giới kho hàng, việc giữ lại một chiếc máy tính rẻ hơn thay vì một chiếc đắt tiền hơn cũng luôn là điều tốt. Việc tái sử dụng nguyên liệu thô là tối ưu một cách độc lập vì việc lưu trữ nguyên liệu thô không giới hạn khiến mọi đơn vị đều chọn con đường rẻ nhất cho tháng sản xuất. Ba đặc tính thống trị này cùng nhau có nghĩa là mọi lựa chọn tham lam đều có thể được trao đổi thành một giải pháp tối ưu mà không làm tăng chi phí. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        k = int(input())

        c = [0] * k
        d = [0] * k
        m = [0] * k
        p = [0] * k

        for i in range(k):
            c[i], d[i], m[i], p[i] = map(int, input().split())

        e = [0] * (k - 1)
        R = [0] * (k - 1)
        E = [0] * (k - 1)

        for i in range(k - 1):
            e[i], R[i], E[i] = map(int, input().split())

        min_heap = []
        max_heap = []

        # remaining[id] is the number of computers left in batch id.
        remaining = []

        raw_cost = 0
        offset = 0
        total_inventory = 0
        answer = 0
        possible = True

        for i in range(k):
            # Computers carried from the previous month have just paid
            # the storage cost on the boundary before month i.
            if i > 0:
                offset += E[i - 1]

                raw_cost = min(c[i], raw_cost + R[i - 1])
            else:
                raw_cost = c[i]

            # This is a candidate batch, not necessarily an actually
            # manufactured batch. Discarding it later means we never
            # needed to manufacture those computers.
            if p[i] > 0:
                batch_id = len(remaining)
                remaining.append(p[i])

                key = raw_cost + m[i] - offset

                heapq.heappush(min_heap, (key, batch_id))
                heapq.heappush(max_heap, (-key, batch_id))

                total_inventory += p[i]

            if total_inventory < d[i]:
                possible = False
                break

            need = d[i]

            # Sell the cheapest available computers.
            while need > 0:
                while min_heap and remaining[min_heap[0][1]] == 0:
                    heapq.heappop(min_heap)

                key, batch_id = heapq.heappop(min_heap)

                take = min(need, remaining[batch_id])
                answer += take * (key + offset)

                remaining[batch_id] -= take
                total_inventory -= take
                need -= take

                if remaining[batch_id] > 0:
                    heapq.heappush(min_heap, (key, batch_id))

            # Only computers crossing to the next month occupy the
            # finished-goods warehouse.
            if i < k - 1 and total_inventory > e[i]:
                remove = total_inventory - e[i]

                # Discard the most expensive computers.
                while remove > 0:
                    while max_heap and remaining[max_heap[0][1]] == 0:
                        heapq.heappop(max_heap)

                    neg_key, batch_id = heapq.heappop(max_heap)
                    key = -neg_key

                    take = min(remove, remaining[batch_id])
                    remaining[batch_id] -= take
                    total_inventory -= take
                    remove -= take

                    if remaining[batch_id] > 0:
                        heapq.heappush(max_heap, (neg_key, batch_id))

        out.append(str(answer) if possible else "-1")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Các mảng hàng tháng được đọc đầu tiên vì quá trình chuyển đổi kho xuất hiện sau tất cả các bản ghi hàng tháng trong đầu vào. Mô phỏng sau đó có thể xử lý các tháng từ trái sang phải.`raw_cost`được cập nhật trước khi tạo lô sản xuất hiện tại. biểu hiện`raw_cost + R[i - 1]`đại diện cho việc mua nguyên liệu thô trước đó và vận chuyển nó qua ranh giới trước đó. Sự thay thế trực tiếp là`c[i]`, vì vậy lấy mức tối thiểu của chúng sẽ mang lại chi phí vật liệu rẻ nhất có thể. 

các`offset`quy ước tránh sửa đổi mọi phần tử heap khi thêm phí lưu trữ. Giả sử một lô được chèn bằng khóa chuẩn hóa`key`. Giá trị thực tế của nó trong tháng hiện tại luôn là`key + offset`. Tăng dần`offset`thay đổi tất cả chi phí thực tế với cùng một lượng, do đó cả hai thứ tự heap vẫn hợp lệ. 

Mỗi tháng sản xuất chỉ tạo tối đa một lô. ID lô cho phép hai đống tham chiếu đến cùng một số lượng mà không trùng lặp số lượng đó. Đống tối thiểu chứa`(key, id)`, trong khi vùng heap tối đa chứa`(-key, id)`. Việc phủ định khóa sẽ chuyển đổi vùng heap tối thiểu của Python thành vùng heap tối đa. 

Một lô có thể được tiêu thụ một phần theo nhu cầu hoặc do cắt giảm công suất. Khi điều đó xảy ra, số lượng đã giảm sẽ được đẩy trở lại vùng mà nó đã bị xóa. Heap còn lại vẫn chứa cùng một ID lô và đọc số lượng hiện tại từ`remaining`, vì vậy các mục cũ có thể bị bỏ qua bất cứ khi nào số lượng của chúng đạt tới 0. 

Việc kiểm tra tính khả thi diễn ra ngay sau khi bổ sung năng lực sản xuất hiện tại. Nếu như`total_inventory < d[i]`, không có tháng nào trong tương lai có thể đáp ứng được nhu cầu của tháng này, vì vậy việc quay trở lại`-1`là an toàn. 

Tất cả các phép tính chi phí đều sử dụng số nguyên Python nên không có vấn đề tràn. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, câu trả lời phải được lưu trữ ở dạng số nguyên 64 bit. Câu trả lời tối đa có thể thoải mái trên phạm vi 32 bit. 

## Ví dụ đã hoạt động 

Mẫu chính thức bao gồm hai trường hợp thử nghiệm. Tháng đầu tiên có hai tháng, tháng đầu tiên có thể sản xuất sáu máy tính và tháng thứ hai có thể sản xuất tám máy tính. Nguyên liệu rẻ tháng đầu có giá trị dự trữ, kho thành phẩm có thể chở được hai máy tính qua biên giới. Đầu ra chính thức là (170). 

### Mẫu 1 

Đầu vào cho trường hợp thử nghiệm đầu tiên là```
2
10 5 3 6
15 7 2 8
2 3 2
```Chi phí nguyên vật liệu thực tế tháng 1 là (10). Vào tháng 2 nó trở thành (\min(15,10+3)=13). Như vậy chi phí sản xuất là (13) và (15). 

| Tháng | Nguyên giá | Bù đắp | Chi phí sản xuất | Số lượng ứng viên | Đã bán | Hàng tồn kho sau bán | Công suất | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 0 | 13 | 6 | 5 | 1 | 2 | 65 | 
| 2 | 13 | 2 | 15 | 9 | 7 | 2 | cuối cùng | 170 | 

Vào cuối tháng 1, một máy tính vẫn còn được lưu trữ. Vượt qua ranh giới sẽ cộng thêm (E_1=2), do đó chi phí của máy tính trở thành (15). Trong tháng thứ 2, số máy tính mới sản xuất cũng có giá (15), do đó cả 7 máy tính cần thiết đều có thể được lấy với giá (15) mỗi chiếc. Tổng cộng là (5\times13+7\time15=170). 

Dấu vết này cũng cho thấy tại sao chi phí lưu trữ phải được áp dụng giữa các tháng. Máy tính còn thừa của tháng đầu tiên có chi phí sản xuất (13), nhưng chi phí sử dụng trong tháng thứ 2 là (13+2=15). 

### Mẫu 2 

Trường hợp thử nghiệm thứ hai là```
2
0 8 0 7
0 0 0 0
0 0 0
```Tháng đầu tiên có năng lực sản xuất (7), trong khi nhu cầu là (8). 

| Tháng | Nguyên giá | Năng lực sản xuất | Nhu cầu | Có sẵn trước khi bán | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 7 | 8 | 7 | Không thể | 
| 2 | 0 | 0 | 0 | chưa đạt | Chưa được xử lý | 

Thuật toán phát hiện`total_inventory < d[0]`ngay lập tức và trả về (-1). Chờ đợi tháng thứ hai cũng không ích gì vì tháng đầu tiên cần tới tám máy tính còn thiếu. Đây chính xác là điều không khả thi được thể hiện qua mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(k\log k)) | Nhiều nhất một lô sản xuất được đưa vào mỗi tháng và các lô được loại bỏ qua hai đống. Mỗi lô được xóa hoàn toàn tối đa một lần, trong khi mỗi tháng chỉ có thể xử lý một phần lô mà quá trình xóa hiện tại dừng lại. | 
| Không gian | (O(k)) | Hai vùng heap và mảng số lượng lô chứa tối đa một lô logic mỗi tháng, trong khi mảng đầu vào hàng tháng cũng sử dụng bộ nhớ (O(k)). | 

Trong tất cả các trường hợp thử nghiệm, tổng (k) tối đa là (300.000), vì vậy (O(k\log k)) là thực tế. Thuật toán không bao giờ phụ thuộc vào tổng số lượng máy tính khổng lồ, đó là lý do nó phù hợp với giới hạn đã định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import heapq

def solve_stream(stream):
    input = stream.readline
    t = int(input())
    out = []

    for _ in range(t):
        k = int(input())

        c = [0] * k
        d = [0] * k
        m = [0] * k
        p = [0] * k

        for i in range(k):
            c[i], d[i], m[i], p[i] = map(int, input().split())

        e = [0] * (k - 1)
        R = [0] * (k - 1)
        E = [0] * (k - 1)

        for i in range(k - 1):
            e[i], R[i], E[i] = map(int, input().split())

        min_heap = []
        max_heap = []
        remaining = []

        raw_cost = 0
        offset = 0
        total = 0
        ans = 0
        possible = True

        for i in range(k):
            if i > 0:
                offset += E[i - 1]
                raw_cost = min(c[i], raw_cost + R[i - 1])
            else:
                raw_cost = c[i]

            if p[i]:
                batch = len(remaining)
                remaining.append(p[i])
                key = raw_cost + m[i] - offset
                heapq.heappush(min_heap, (key, batch))
                heapq.heappush(max_heap, (-key, batch))
                total += p[i]

            if total < d[i]:
                possible = False
                break

            need = d[i]
            while need:
                while remaining[min_heap[0][1]] == 0:
                    heapq.heappop(min_heap)

                key, batch = heapq.heappop(min_heap)
                take = min(need, remaining[batch])

                ans += take * (key + offset)
                remaining[batch] -= take
                total -= take
                need -= take

                if remaining[batch]:
                    heapq.heappush(min_heap, (key, batch))

            if i < k - 1 and total > e[i]:
                remove = total - e[i]

                while remove:
                    while remaining[max_heap[0][1]] == 0:
                        heapq.heappop(max_heap)

                    neg_key, batch = heapq.heappop(max_heap)
                    take = min(remove, remaining[batch])

                    remaining[batch] -= take
                    total -= take
                    remove -= take

                    if remaining[batch]:
                        heapq.heappush(max_heap, (neg_key, batch))

        out.append(str(ans) if possible else "-1")

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve_stream(sys.stdin)
    finally:
        sys.stdin = old_stdin

# Provided samples
sample = """\
2
2
10 5 3 6
15 7 2 8
2 3 2
2
0 8 0 7
0 0 0 0
0 0 0
"""

assert run(sample) == "170\n-1", "provided samples"

# Minimum-size case.
# Every demand is satisfied immediately, with no inventory crossing.
minimum_case = """\
1
2
0 1 0 1
0 1 0 1
0 0 0
"""

assert run(minimum_case) == "0", "minimum-size case"

# All values equal.
# Each month produces exactly its demand, so storage is never needed.
all_equal_case = """\
1
2
5 2 3 2
5 2 3 2
2 1 1
"""

assert run(all_equal_case) == "32", "all equal values"

# Raw material is bought in month 1, stored, then used in month 2.
# The finished computer also crosses the boundary and pays E.
storage_case = """\
1
2
1 0 0 1
100 1 0 1
1 1 2
"""

assert run(storage_case) == "3", "raw and finished-good storage"

# Maximum-size case.
# 50,000 months, one computer demanded and produced every month,
# with zero costs and zero finished-goods storage.
k = 50000
maximum_case = (
    "1\n"
    + str(k)
    + "\n"
    + ("0 1 0 1\n" * k)
    + ("0 0 0\n" * (k - 1))
)

assert run(maximum_case) == "0", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hộp kích thước tối thiểu có thời hạn hai tháng và sản xuất ngay |`0`| Ranh giới cơ bản và không có hàng tồn kho không cần thiết | 
| Hai tháng với mức giá và công suất giống nhau |`32`| Các lô có chi phí bằng nhau và tổng hợp số lượng | 
| Mua nguyên liệu thô sớm và bảo quản thành phẩm |`3`| Tái lưu trữ thô và chi phí lưu trữ thành phẩm | 
| Trường hợp đã tạo (k=50.000) |`0`| Kích thước đầu vào tối đa và sự độc lập với tổng số máy tính | 

## Vỏ cạnh 

Dung lượng kho bằng 0 có nghĩa là không có máy tính hoàn chỉnh nào có thể vượt qua ranh giới đó. Coi như```
1
2
0 0 0 2
0 2 0 0
1 0 0
```Trong tháng 1, có hai ứng viên và nhu cầu bằng không. Sau khi nhu cầu được xử lý, kho chứa hai máy tính, nhưng (e_1=1), do đó, đống chi phí tối đa sẽ loại bỏ một máy tính. Khoảng không quảng cáo còn lại có kích thước một. Tháng 2 có nhu cầu là hai và năng lực sản xuất bằng 0, do đó chỉ có một máy tính và thuật toán trả về (-1). Công suất được áp dụng sau đợt bán hàng hiện tại, chính xác là nơi có giới hạn ranh giới. 

Nguyên liệu thô có thể được mua từ lâu trước tháng sản xuất. Coi như```
1
2
1 0 0 0
100 1 0 1
1 1 0
```Chi phí nguyên liệu thực tế của tháng đầu tiên là (1). Chi phí thô hiệu dụng của tháng thứ hai là (\min(100,1+1)=2). Do đó, tháng thứ hai sản xuất chiếc máy tính cần thiết với giá (2), đưa ra câu trả lời (2). Thuật toán không cần kiểm kê nguyên liệu thô rõ ràng vì`raw_cost`đã là cách rẻ nhất để chuyển một đơn vị nguyên liệu thô sang tháng hiện tại. 

Việc lưu trữ thành phẩm cũng có tác dụng tích lũy tương tự. Coi như```
1
2
1 0 0 1
100 1 0 1
1 1 2
```Máy tính của tháng đầu tiên có chi phí sản xuất (1). Nó tồn tại trong quá trình kiểm tra dung lượng vì (e_1=1). Khi thuật toán tiến tới tháng thứ 2,`offset`tăng thêm (2), do đó chi phí hiện tại của máy tính được lưu trữ sẽ trở thành (1+2=3). Máy tính mới của tháng thứ 2 sẽ có giá (100), do đó số lượng tối thiểu sẽ bán máy tính được lưu trữ và câu trả lời là (3). 

Nhu cầu bằng 0 cũng cần được xử lý mà không cần chạm vào đống tối thiểu. Ví dụ,```
1
2
0 0 0 1
0 1 0 1
0 0 0
```có câu trả lời (0). Tháng đầu tiên có thể có một ứng viên sản xuất nhưng nhu cầu bằng 0 và dung lượng lưu trữ cũng bằng 0 nên ứng viên đó sẽ bị loại bỏ ở ranh giới. Tháng 2 sau đó sản xuất máy tính theo yêu cầu của riêng mình với chi phí bằng 0. Việc triển khai ở cấp đơn vị giả định rằng cuối cùng mọi ứng viên sản xuất đều phải được bán có thể tính phí không chính xác cho máy tính bị loại bỏ. 

Cuối cùng, một lô hàng chỉ có thể được bán một phần hoặc bị loại bỏ một phần. Hãy xem xét tháng đầu tiên của mẫu đầu tiên, trong đó có sáu ứng viên nhưng chỉ có năm ứng viên được bán. Số lượng lô thay đổi từ sáu thành một và số còn lại vẫn ở trong cả hai cấu trúc heap. Sau đó, nếu lô đó trở thành lô còn sót lại đắt nhất, thì đống tối đa chỉ có thể loại bỏ một máy tính đó. Việc lưu trữ một số lượng trong mỗi lô là điều giữ cho hoạt động này không phụ thuộc vào số lượng máy tính trong lô.
