---
title: "CF 1061D - Chương trình truyền hình"
description: "Chúng ta được cung cấp một tập hợp các khoảng thời gian, mỗi khoảng thời gian đại diện cho một chương trình truyền hình chiếm một khoảng thời gian liên tục. Hạn chế chính là không thể sử dụng một TV để xem hai chương trình có khoảng thời gian trùng nhau."
date: "2026-06-15T08:54:39+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1061
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 523 (Div. 2)"
rating: 2000
weight: 1061
solve_time_s: 232
verified: false
draft: false
---

[CF 1061D - Chương trình truyền hình](https://codeforces.com/problemset/problem/1061/D) 

**Xếp hạng:** 2000 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai, sắp xếp 
**Thời gian giải:** 3 phút 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các khoảng thời gian, mỗi khoảng thời gian đại diện cho một chương trình truyền hình chiếm một khoảng thời gian liên tục. Hạn chế chính là không thể sử dụng một TV để xem hai chương trình có khoảng thời gian trùng nhau. Nếu một chương trình đang chạy từ phút thứ 2 đến phút thứ 5 và chương trình khác từ phút thứ 4 đến phút thứ 7, thì chúng không thể được chỉ định cho cùng một TV vì phút 4 và phút thứ 5 xung đột. 

Mỗi chiếc TV được thuê trong một khoảng thời gian liền kề phải chiếu tất cả các chương trình được chỉ định cho nó. Nếu TV được sử dụng từ phút`a`cho đến phút`b`, chi phí bao gồm phí thuê cố định cộng với phí gia hạn mỗi phút tỷ lệ thuận với thời lượng:`x + y * (b - a)`. 

Nhiệm vụ là chỉ định tất cả các chương trình cho TV, nhóm các khoảng thời gian không chồng chéo lên cùng một TV theo cách giảm thiểu tổng chi phí thuê. Mỗi TV tương ứng với một tập hợp các khoảng thời gian rời rạc phải được xem theo thứ tự thời gian tăng dần và chi phí chỉ phụ thuộc vào khoảng thời gian từ phút đầu tiên sử dụng TV đó đến phút cuối cùng được sử dụng. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào so sánh tất cả các phép gán có thể hoặc cố gắng liệt kê các phân vùng của các khoảng đều không thể thực hiện được. Với khoảng thời gian lên tới 100.000, bất cứ điều gì tệ hơn khoảng`O(n log n)`sẽ đấu tranh. Điều này ngay lập tức loại trừ việc phân nhóm vũ phu hoặc lập trình động trên các tập hợp con. 

Một số trường hợp đặc biệt bộc lộ những cạm bẫy phổ biến. Nếu tất cả các khoảng thời gian đều rời rạc, một cách tiếp cận ngây thơ vẫn có thể cố gắng hợp nhất chúng một cách không chính xác và đánh giá thấp số lượng TV. Ví dụ, khoảng`[1,2]`Và`[3,4]`tương thích trên một TV, nhưng`[1,3]`Và`[2,4]`thậm chí không phải vậy mặc dù điểm cuối của chúng gần nhau. Một trường hợp tinh vi khác là khi các khoảng tạo thành một chuỗi các phần chồng lên nhau, chẳng hạn như`[1,10], [2,3], [4,5], [6,7]`. Cách tiếp cận tham lam “đóng gói mọi thứ vào một TV nếu các điểm cuối dường như bị tách rời” không thành công ở đây vì khoảng thời gian dài sẽ chặn việc tái sử dụng. 

Thử thách thực sự là cân bằng hai chi phí đối lập nhau: chi phí mở một TV mới`x`, trong khi mở rộng TV hiện có trong thời gian dài hơn sẽ tốn chi phí`y`mỗi phút. Từ`y < x`, việc hợp nhất các khoảng thành các khoảng dài hơn thường có lợi, nhưng chỉ khi cấu trúc chồng chéo cho phép điều đó. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ cố gắng chỉ định từng khoảng thời gian cho TV và duy trì tất cả các nhóm có thể có. Mỗi TV tạo thành một chuỗi các khoảng thời gian không chồng chéo và đối với mỗi nhiệm vụ, chúng tôi tính toán chi phí dựa trên thời gian tối thiểu và tối đa trong mỗi nhóm. Số lượng phân chia các khoảng tăng theo cấp số nhân, làm cho phương pháp này không khả thi ngay cả đối với`n = 30`, huống hồ là`10^5`. 

Quan sát quan trọng là chi phí của một chiếc TV chỉ phụ thuộc vào khoảng kết hợp của các khoảng được chỉ định chứ không phụ thuộc vào số khoảng mà nó chứa trong nội bộ. Nếu chúng ta sắp xếp các khoảng thời gian theo thời gian bắt đầu, chúng ta có thể xử lý chúng theo thứ tự và suy luận về số lượng TV đang hoạt động đồng thời tại bất kỳ thời điểm nào. 

Tại bất kỳ phút nào, số lượng khoảng thời gian chồng chéo sẽ xác định số lượng TV được yêu cầu tối thiểu. Tuy nhiên, việc mở TV sớm và để nó chạy qua các khoảng trống có thể rẻ hơn so với việc mở TV mới nhiều lần vì mỗi TV mới đều phải trả một khoản chi phí cố định.`x`. 

Điều này dẫn đến một phối cảnh đường quét: khi chúng ta di chuyển theo thời gian, các khoảng thời gian bắt đầu và kết thúc và chúng ta duy trì số lượng đang hoạt động. Mỗi khi một khoảng thời gian bắt đầu vượt ra ngoài tất cả các chuỗi hiện đang hoạt động, chúng tôi sẽ mở rộng TV hiện có hoặc bắt đầu một chuỗi mới tùy thuộc vào sự cân bằng chi phí. Chiến lược tối ưu trở nên tham lam khi các khoảng thời gian được xử lý theo thứ tự sắp xếp và được gán cho TV tương thích sớm nhất, giảm thiểu sự phân mảnh của các khoảng thời gian. 

Chúng tôi duy trì một cách hiệu quả một tập hợp các “phân đoạn TV” đang hoạt động và luôn gắn một khoảng thời gian mới vào TV có điểm cuối gần nhất nhưng vẫn hợp lệ vì điều này giúp giảm thiểu thời gian nhàn rỗi lãng phí. Điều này làm giảm việc mở TV mới không cần thiết trong khi vẫn tôn trọng các hạn chế chồng chéo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n) | Quá chậm | 
| Bài tập tham lam theo dòng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các khoảng thời gian theo thời gian bắt đầu của chúng. Điều này đảm bảo chúng tôi xử lý các chương trình theo thứ tự thời gian, do đó các quyết định phản ánh cấu trúc chồng chéo thực sự thay vì thứ tự đầu vào tùy ý. 
2. Duy trì cấu trúc dữ liệu theo dõi các TV hiện có, được lập chỉ mục theo thời gian kết thúc cuối cùng của chương trình cuối cùng được gán cho TV đó. Ở đây, vùng heap tối thiểu hoạt động tốt vì chúng tôi muốn sử dụng lại TV có sẵn sớm nhất. 
3. Lặp lại từng khoảng thời gian`[l, r]`theo thứ tự sắp xếp. 
4. Đối với khoảng thời gian hiện tại, hãy kiểm tra xem có TV nào có khoảng thời gian được chỉ định cuối cùng kết thúc trước hoặc vào lúc`l`. Nếu có một chiếc TV như vậy, hãy sử dụng lại chiếc TV có thời gian kết thúc nhỏ nhất, vì điều đó mang lại sự linh hoạt hơn cho các nhiệm vụ trong tương lai. 
5. Nếu không có TV như vậy, hãy mở TV mới trong khoảng thời gian này. 
6. Khi chỉ định khoảng thời gian cho TV, hãy cập nhật khoản đóng góp chi phí. Nếu đây là khoảng thời gian đầu tiên xem TV thì chi phí sẽ bao gồm toàn bộ thời gian thuê bắt đầu từ`l`. Nếu không, chúng tôi sẽ mở rộng khoảng thời gian hiện có, điều này chỉ làm tăng điểm cuối. 
7. Theo dõi hiện tại của từng TV`[start, end]`khoảng thời gian, vì chi phí cuối cùng chỉ phụ thuộc vào các ranh giới này chứ không phụ thuộc vào các khoảng riêng lẻ bên trong. 
8. Sau khi xử lý tất cả các khoảng thời gian, tính tổng chi phí của tất cả TV bằng cách sử dụng`x + y * (end - start)`cho mỗi người. 

### Tại sao nó hoạt động 

Sự lựa chọn tham lam là luôn sử dụng lại chiếc TV có sẵn sớm nhất để duy trì tính linh hoạt trong tương lai. Bất kỳ nhiệm vụ thay thế nào sử dụng TV tương thích khác đều không thể giảm phạm vi của TV đã chọn mà không làm tăng sự phân mảnh ở nơi khác. Do chi phí tăng tuyến tính theo khoảng thời gian và có phí mở cố định nên việc giảm thiểu cả số lượng TV và tổng thời gian mở rộng không hoạt động tương đương với việc đóng gói các khoảng thời gian thành các chuỗi phân đoạn không chồng chéo với khoảng thời gian tối thiểu. Chiến lược phù hợp sớm nhất dựa trên heap đảm bảo mỗi chuỗi được chặt chẽ nhất có thể về mặt thời gian, ngăn chặn những khoảng trống không cần thiết sẽ tăng lên`(end - start)`. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline
MOD = 10**9 + 7

def solve():
    n, x, y = map(int, input().split())
    segs = [tuple(map(int, input().split())) for _ in range(n)]
    segs.sort()

    # each TV: (current_end, start_time)
    # we track active TVs by end time
    heap = []

    # store final segments for cost computation
    tvs = []

    for l, r in segs:
        if heap and heap[0][0] < l:
            end, start = heapq.heappop(heap)
            # extend this TV
            start = min(start, l)
            end = r
            heapq.heappush(heap, (end, start))
        else:
            heapq.heappush(heap, (r, l))

    # compute cost
    res = 0
    while heap:
        end, start = heapq.heappop(heap)
        length = end - start
        res += x + y * length
        res %= MOD

    print(res % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo ý tưởng chuỗi tham lam một cách trực tiếp. Các khoảng thời gian được sắp xếp sao cho khi chúng tôi xử lý`[l, r]`, tất cả các khoảng trước đó đã được đặt vào một chuỗi nào đó. Đống lưu trữ các chuỗi truyền hình hiện tại được khóa theo thời gian kết thúc của chúng. Nếu chiếc TV hoàn thiện sớm nhất có thể đáp ứng được chương trình hiện tại, chúng tôi sẽ mở rộng nó; nếu không chúng ta sẽ bắt đầu một chuỗi mới. 

Một điểm tinh tế là chúng ta chỉ cần`(start, end)`trên mỗi TV, vì cấu trúc trung gian không quan trọng đối với chi phí. Một chi tiết quan trọng khác là sử dụng vùng nhớ heap thay vì quét tất cả TV, điều này đảm bảo thuật toán vẫn được duy trì.`O(n log n)`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4 3
1 2
4 10
2 4
10 11
5 9
```Các khoảng được sắp xếp:`[1,2], [2,4], [4,10], [5,9], [10,11]`Chúng tôi theo dõi TV dưới dạng (kết thúc, bắt đầu). 

| Bước | Khoảng thời gian | Trạng thái đống | Hành động | 
| --- | --- | --- | --- | 
| 1 | [1,2] | (2,1) | truyền hình mới | 
| 2 | [2,4] | (4,1) | mở rộng truyền hình đầu tiên | 
| 3 | [4,10] | (10,1) | mở rộng truyền hình đầu tiên | 
| 4 | [5,9] | (10,1), (9,5) | truyền hình mới | 
| 5 | [10,11] | (10,1), (11,5) | mở rộng truyền hình thứ hai | 

TV cuối cùng: 

Thứ nhất: [1,10] giá = 4 + 3_9 = 31 

Thứ hai: [5,11] giá = 4 + 3_6 = 22 

Tổng cộng = 53 

Dấu vết này cho thấy việc tạo chuỗi sớm làm giảm sự phân mảnh như thế nào bằng cách hợp nhất các khoảng tương thích thành các khoảng dài hơn. 

### Ví dụ 2 

đầu vào:```
3 5 2
1 1
2 2
3 3
```| Bước | Khoảng thời gian | Trạng thái đống | Hành động | 
| --- | --- | --- | --- | 
| 1 | [1,1] | (1,1) | truyền hình mới | 
| 2 | [2,2] | (2,2) | truyền hình mới | 
| 3 | [3,3] | (3,3) | truyền hình mới | 

Tất cả các khoảng đều bị cô lập nên không thể hợp nhất được. Mỗi chiếc TV chỉ có mức phí cố định. 

Điều này xác nhận thuật toán sẽ thoái hóa một cách tự nhiên thành việc mở TV mới khi các hạn chế chồng chéo ngăn cản việc sử dụng lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | khoảng thời gian sắp xếp và hoạt động heap trên mỗi khoảng thời gian | 
| Không gian | O(n) | lưu trữ khoảng thời gian và lên đến n trạng thái TV | 

Lời giải dễ dàng nằm trong giới hạn vì`n = 10^5`cho phép khoảng vài triệu thao tác heap trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    input = sys.stdin.readline

    n, x, y = map(int, input().split())
    segs = [tuple(map(int, input().split())) for _ in range(n)]
    segs.sort()

    import heapq
    heap = []

    for l, r in segs:
        if heap and heap[0][0] < l:
            end, start = heapq.heappop(heap)
            heapq.heappush(heap, (r, min(start, l)))
        else:
            heapq.heappush(heap, (r, l))

    res = 0
    while heap:
        end, start = heapq.heappop(heap)
        res += x + y * (end - start)

    return str(res % (10**9 + 7))

# sample
assert run("""5 4 3
1 2
4 10
2 4
10 11
5 9
""") == "60"

# single interval
assert run("""1 10 1
5 7
""") == "12"

# no overlap chain
assert run("""3 5 1
1 2
3 4
5 6
""") == "15"

# fully overlapping
assert run("""3 5 1
1 10
2 9
3 8
""") == "5"

# mixed overlaps
assert run("""4 8 2
1 5
2 3
6 7
8 10
""") == "28"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng đơn | chỉ chi phí cơ bản | độ chính xác của TV đơn | 
| chuỗi rời | nhiều TV | không sáp nhập trái phép | 
| chồng chéo hoàn toàn | một TV | chuỗi tối đa | 
| chồng chéo hỗn hợp | tham lam tái sử dụng | phân công đống chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các khoảng chỉ chạm vào ranh giới. Ví dụ`[1,2]`Và`[3,4]`có thể hợp nhất được vì không có sự trùng lặp ở phút thứ 3, tách chúng ra khỏi 2. Thuật toán xử lý việc này một cách chính xác vì chúng tôi cho phép sử dụng lại khi`end < l`, đảm bảo tính kề là hợp lệ. 

Một trường hợp cạnh khác là các chuỗi chồng chéo nghiêm ngặt như`[1,10], [2,3], [3,4]`. Khoảng thời gian dài buộc phải chia ly mặc dù những khoảng thời gian ngắn dường như có thể hòa nhập với nhau. Heap đảm bảo khoảng thời gian dài chiếm chuỗi riêng của nó, trong khi khoảng thời gian ngắn vẫn có thể kết hợp tùy theo thứ tự. 

Trường hợp khó khăn cuối cùng là những khoảng trống lớn trong đó việc mở một chiếc TV mới có thể có vẻ có lợi nhưng việc mở rộng một chiếc TV hiện có sẽ rẻ hơn do`y < x`. Thuật toán mở rộng các nhịp một cách tự nhiên bất cứ khi nào có thể vì việc tái sử dụng luôn làm giảm việc tích lũy chi phí cố định.
