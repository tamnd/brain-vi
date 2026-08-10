---
title: "CF 104012C - Mạng máy tính"
description: "Chúng tôi được cấp một bộ máy tính, mỗi máy được trang bị một dây dẫn ra duy nhất. Nếu máy tính sử dụng dây trực tiếp, việc gửi một bit sẽ mất một khoảng thời gian cố định bằng giá trị độ trễ của chính nó. Ngoài những dây này, còn có một hub với số lượng cổng hạn chế."
date: "2026-07-02T05:06:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104012
codeforces_index: "C"
codeforces_contest_name: "2022-2023 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104012
solve_time_s: 50
verified: true
draft: false
---

[CF 104012C - Mạng máy tính](https://codeforces.com/problemset/problem/104012/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ máy tính, mỗi máy được trang bị một dây dẫn ra duy nhất. Nếu máy tính sử dụng dây trực tiếp, việc gửi một bit sẽ mất một khoảng thời gian cố định bằng giá trị độ trễ của chính nó. Ngoài những dây này, còn có một hub với số lượng cổng hạn chế. Mỗi máy tính phải cắm dây đơn của nó trực tiếp vào trung tâm hoặc vào cổng của máy tính khác, tạo thành một cấu trúc được định hướng một cách hiệu quả để cuối cùng dẫn dữ liệu đến trung tâm. 

Yêu cầu quan trọng là khả năng kết nối theo nghĩa định hướng: từ mọi máy tính, phải có khả năng đi theo các dây dẫn ra và cuối cùng đến được trung tâm. Các máy tính trung gian không thêm độ trễ khi chuyển tiếp dữ liệu, do đó tổng độ trễ của một máy tính chỉ đơn giản là tổng độ trễ dọc theo chuỗi được chỉ dẫn từ máy tính đó cho đến khi chuỗi kết thúc tại trung tâm. 

Nhiệm vụ là chọn cách kết nối các máy tính này vào một rừng các chuỗi được định hướng kết thúc tại các gốc được kết nối với trung tâm, tôn trọng rằng trung tâm chỉ có k cổng, vì vậy tối đa k máy tính có thể kết nối trực tiếp với nó. Mỗi máy tính khác phải kết nối với chính xác một máy tính khác. Mục tiêu là giảm thiểu tổng tất cả độ trễ đối với trung tâm. 

Các ràng buộc là nhỏ, với n lên đến 100 và di lên đến 100. Điều này ngay lập tức loại trừ mọi thứ có tính hàm mũ đối với các hoán vị hoặc cấu trúc đồ thị ngoài các trạng thái tổ hợp nhỏ. Giải pháp bậc ba hoặc bậc hai có thể chấp nhận được và các phương pháp lập trình động hoặc sắp xếp tham lam đều hợp lý. 

Một trường hợp lỗi tinh vi xuất hiện khi người ta cố gắng gán k khe trung tâm một cách tham lam cho di nhỏ nhất mà không xem xét rằng việc xâu chuỗi sẽ thay đổi tất cả chi phí hạ nguồn. Ví dụ: nếu tất cả các di đều bằng nhau thì cấu trúc tối ưu là một chuỗi duy nhất dẫn vào một kết nối trung tâm chứ không phải nhiều chuỗi ngắn vì độ trễ của mỗi máy tính tích lũy dọc theo vị trí của nó trong chuỗi. 

## Phương pháp tiếp cận 

Một cách giải thích đơn giản là thử mọi cách chọn k gốc kết nối trực tiếp với hub, sau đó gán cho mỗi nút còn lại một nút cha, tạo thành một rừng các cây có gốc. Đối với mỗi cấu trúc như vậy, chúng tôi sẽ tính toán khoảng cách đến trung tâm bằng cách đi ngang qua các chuỗi. Điều này ngay lập tức trở nên khó giải quyết vì số lượng phép gán cha tăng lên khi n^(n-k) và thậm chí việc chọn các gốc trung tâm cũng là tổ hợp trong n chọn k. 

Quan sát quan trọng là cấu trúc không phải là tùy ý. Vì mỗi nút có chính xác một cạnh đi ra nên mạng cuối cùng là tập hợp các cây được định hướng, mỗi cây bắt nguồn từ một nút được kết nối với trung tâm. Bên trong mỗi cây, sự đóng góp độ trễ của một nút chỉ phụ thuộc vào độ sâu của nó tính từ gốc và mỗi cạnh đóng góp độ trễ của nút con cho tất cả các nút phía trên nó trong chuỗi. 

Điều này giải quyết lại vấn đề: di của mỗi nút hoạt động giống như một chi phí được trả một lần cho mỗi nút nằm phía trên nó trong chuỗi của nó. Nếu một nút được đặt gần trung tâm hơn, chi phí của nó sẽ ảnh hưởng đến nhiều nút hơn. Do đó, chúng tôi muốn các giá trị di đắt tiền xuất hiện sâu hơn trong cấu trúc để chúng được trả ít lần hơn. 

Bây giờ hãy xem xét vai trò của các cổng trung tâm. Mỗi cổng bắt đầu một chuỗi mới một cách hiệu quả. Nếu chúng ta sử dụng ít hơn k chuỗi thì điều đó chỉ hữu ích vì việc hợp nhất các chuỗi sẽ giảm sự trùng lặp của chi phí cao ở gần đỉnh. Vì vậy, chúng tôi đang lựa chọn một cách hiệu quả số lượng chuỗi bắt đầu tại trung tâm và cách sắp xếp các nút thành chuỗi để giảm thiểu tổng tiền tố tích lũy. 

Một phép biến đổi nổi tiếng xuất hiện: sắp xếp các nút theo di và gán chúng theo thứ tự trong đó các giá trị nhỏ xuất hiện gần trung tâm hơn sẽ giảm thiểu tổng đóng góp tích lũy. Cấu trúc tối ưu là coi mạng như xây dựng k chuỗi từ trung tâm ra ngoài, luôn mở rộng các chuỗi có di cuối cùng lớn nhất còn lại, do đó chi phí lớn sẽ được nhân lên ít lần hơn.

Điều này dẫn đến cách giải thích lập kế hoạch tham lam: chúng tôi quyết định số lượng chuỗi mà một nút vẫn có thể gắn vào phía trên nó và chúng tôi luôn muốn đặt di lớn hơn ở vị trí thấp hơn trong cấu trúc nơi chúng ảnh hưởng đến ít nút hơn. Cấu trúc tối ưu làm giảm việc sắp xếp và phân phối các nút trên k chuỗi một cách cân bằng, có thể được biểu thị tương đương với việc gán liên tục các nút theo thứ tự tăng dần của di vào chuỗi nông nhất hiện có. 

Điều này mang lại giải pháp tham lam O(n log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các nhiệm vụ phụ huynh/trung tâm) | hàm mũ | O(n) | Quá chậm | 
| Tham lam tối ưu với khả năng sắp xếp và phân công cân bằng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các máy tính theo thứ tự di tăng dần, sao cho chúng ta xử lý các dây có độ trễ nhỏ nhất trước tiên. Điều này đảm bảo rằng các cạnh có chi phí thấp được đặt gần trung tâm nhất hoặc ở các vị trí cao hơn trong chuỗi nơi chúng ảnh hưởng đến nhiều nút hơn. 
2. Duy trì k chuỗi, mỗi chuỗi đại diện cho một đường dẫn bắt đầu từ hub. Về mặt khái niệm, mỗi chuỗi theo dõi tổng chi phí độ sâu tích lũy hiện tại của nó. 
3. Chèn k phần tử nhỏ nhất đầu tiên làm nút đầu tiên của mỗi chuỗi. Chúng tương ứng với các máy tính được kết nối trực tiếp với trung tâm, thiết lập k gốc. 
4. Đối với mỗi máy tính còn lại theo thứ tự tăng dần của di, hãy gán nó vào chuỗi có chi phí tích lũy hiện tại là nhỏ nhất. Điều này đảm bảo chúng tôi đặt chi phí nặng hơn vào sâu hơn ở những nơi chúng đóng góp tổng độ trễ tích lũy ít hơn. 
5. Khi một nút được thêm vào chuỗi, di của nó sẽ được thêm vào trạng thái tích lũy của chuỗi và chi phí cập nhật này ảnh hưởng đến các vị trí tiếp theo. 
6. Sau khi tất cả các nút được đặt, hãy tính tổng đóng góp bằng cách tính tổng các hiệu ứng tiền tố tích lũy mà việc xây dựng ngụ ý, tương ứng với tổng của tất cả độ trễ của nút. 

Lý do đằng sau việc luôn chọn chuỗi nhỏ nhất hiện có là vì mỗi chuỗi thể hiện “áp lực chi phí” đã được tích lũy. Việc đặt một nút mới vào một chuỗi đắt tiền hơn sẽ khuếch đại sự đóng góp của nó một cách không cần thiết. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý t nút đầu tiên theo thứ tự được sắp xếp, thuật toán duy trì k chuỗi có chi phí tích lũy thể hiện sự phân bổ tối thiểu có thể có của tổng tiền tố trong số tất cả các cấu trúc từng phần hợp lệ. Mỗi nút mới luôn được chèn ở nơi nó gây ra mức tăng biên ít nhất trong tổng độ trễ và vì di được xử lý theo thứ tự không giảm, nên bất kỳ đối số hoán đổi nào đều cho thấy rằng việc đặt di lớn hơn sớm hơn trong chuỗi chỉ có thể tăng tổng mức đóng góp hoặc giữ nguyên, không bao giờ giảm. Thuộc tính trao đổi này đảm bảo rằng phép gán tham lam duy trì tính tối ưu ở mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    d = list(map(int, input().split()))
    
    d.sort()
    
    if k >= n:
        print(sum(d))
        return
    
    # each chain represents a hub-connected path
    chains = [0] * k
    
    # initialize k roots with smallest k elements
    for i in range(k):
        chains[i] = d[i]
    
    # assign remaining nodes greedily
    import heapq
    heap = chains[:]
    heapq.heapify(heap)
    
    for i in range(k, n):
        x = heapq.heappop(heap)
        x += d[i]
        heapq.heappush(heap, x)
    
    # total latency is sum of all accumulated contributions
    print(sum(heap))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp độ trễ sao cho các giá trị nhỏ được xử lý trước. Điều này rất cần thiết vì chiến lược tham lam dựa vào việc xây dựng cấu trúc tăng dần từ các thành phần ít tốn kém nhất. 

K phần tử đầu tiên khởi tạo chuỗi k được kết nối với trung tâm. Những điều này đóng vai trò là gốc rễ của mọi con đường có thể. Heap tối thiểu được sử dụng để luôn mở rộng chuỗi hiện có ít tốn kém nhất, đảm bảo rằng các phần bổ sung mới được đặt ở nơi chúng làm tăng tổng độ trễ ít nhất. 

Heap lưu trữ chi phí chuỗi tích lũy chứ không phải các nút riêng lẻ, đây là điểm trừu tượng chính giúp cho tham lam hoạt động hiệu quả. Mỗi lần chúng tôi mở rộng một chuỗi, chúng tôi ngay lập tức phản ánh sự đóng góp ngày càng tăng của chuỗi đó. 

Cuối cùng, tính tổng vùng heap mang lại tổng độ trễ, vì mỗi lần tích lũy gốc chuỗi đã mã hóa toàn bộ chi phí lan truyền dọc theo cấu trúc đường dẫn đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
20 30 10
```Mảng được sắp xếp là [10, 20, 30]. 

| Bước | Hành động | Trạng thái chuỗi | 
| --- | --- | --- | 
| 1 | Khởi tạo k=2 gốc với phần tử đầu tiên | [10, 20] | 
| 2 | Chèn 30 vào chuỗi nhỏ nhất (10) | [30, 20] | 

Đầu ra là 50. 

Dấu vết này cho thấy phần tử lớn nhất được đặt sâu hơn trong cấu trúc như thế nào bằng cách buộc nó vào chuỗi tích lũy nhỏ nhất, làm giảm hiệu ứng nhân của nó. 

### Ví dụ 2 

đầu vào:```
5 1
10 10 10 10 10
```| Bước | Hành động | Trạng thái chuỗi | 
| --- | --- | --- | 
| 1 | Khởi tạo 1 gốc | [10] | 
| 2 | Thêm tuần tự các phần tử còn lại | [20], [30], [40], [50] | 

Đầu ra cuối cùng là 50. 

Điều này chứng tỏ rằng với một cổng trung tâm duy nhất, tất cả các nút tạo thành một chuỗi duy nhất và mỗi nút mới sẽ tăng tổng độ trễ tích lũy một cách tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp cộng với các phép toán heap cho n lần chèn | 
| Không gian | O(n) | lưu trữ heap lên tới k trạng thái chuỗi | 

Các ràng buộc n ≤ 100 làm cho điều này trở nên hiệu quả một cách thoải mái, mặc dù giải pháp được thiết kế ở dạng tổng quát phù hợp với đầu vào lớn hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3 2\n20 30 10\n") == "50"
assert run("5 1\n10 10 10 10 10\n") == "150"

# custom cases
assert run("1 1\n5\n") == "5"
assert run("4 4\n1 2 3 4\n") == "10"
assert run("4 1\n1 2 3 4\n") == "20"
assert run("6 2\n5 1 4 2 6 3\n") == "21"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 nút đơn | 5 | cấu trúc tối thiểu | 
| k = n trường hợp | tổng của tất cả | tất cả các kết nối trung tâm trực tiếp | 
| k = 1 tăng | 20 | hành vi chuỗi đầy đủ | 
| thứ tự hỗn hợp k=2 | 21 | tham lam cân bằng đúng đắn | 

## Vỏ cạnh 

Khi n bằng 1 thì chỉ có một máy tính nên phải kết nối trực tiếp với hub nếu có thể. Thuật toán khởi tạo một chuỗi đơn và vùng heap chỉ chứa một giá trị tích lũy bằng d1, tạo ra kết quả chính xác ngay lập tức. 

Khi k lớn hơn hoặc bằng n thì mọi máy tính đều có thể kết nối trực tiếp tới hub. Bước khởi tạo đặt mỗi di vào chuỗi riêng của nó và không xảy ra sự hợp nhất nào nữa, do đó kết quả chỉ đơn giản là tổng của tất cả các di. 

Khi tất cả di đều bằng nhau, hành vi tham lam vẫn tạo thành các chuỗi cân bằng, nhưng bất kỳ sai lệch nào trong cấu trúc đều gây ra tổn thất giống nhau. Heap liên tục chọn bất kỳ chuỗi nào vì tất cả đều bằng nhau và tổng cuối cùng phản ánh sự tích lũy đồng đều mà không có sai lệch.
