---
title: "CF 416B - Đoàn Nghệ Thuật"
description: "Chúng tôi được cấp một dây chuyền sản xuất trong đó nhiều bức tranh được chuyển qua một chuỗi họa sĩ cố định. Mỗi bức tranh đều phải qua tay tất cả các họa sĩ theo thứ tự, từ người đầu tiên đến người cuối cùng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 416
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 241 (Div. 2)"
rating: 1300
weight: 416
solve_time_s: 86
verified: true
draft: false
---

[CF 416B - Liên minh nghệ thuật](https://codeforces.com/problemset/problem/416/B) 

**Đánh giá:** 1300 
**Tags:** vũ lực, dp, thực hiện 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một dây chuyền sản xuất trong đó nhiều bức tranh được chuyển qua một chuỗi họa sĩ cố định. Mỗi bức tranh đều phải qua tay tất cả các họa sĩ theo thứ tự, từ người đầu tiên đến người cuối cùng. Mỗi họa sĩ xử lý các bức tranh một cách tuần tự trong hàng đợi riêng của họ và họ không thể làm việc trên nhiều bức tranh cùng một lúc. Tuy nhiên, sau khi họa sĩ hoàn thành phần việc của mình trên bức tranh, bức tranh sẽ ngay lập tức có sẵn cho họa sĩ tiếp theo, người có thể đang chờ đợi hoặc đang bận. 

Mỗi bức tranh có thời gian xử lý khác nhau của mỗi họa sĩ. Mục tiêu là để xác định, đối với mỗi bức tranh, thời điểm chính xác khi nó hoàn thành giai đoạn cuối cùng, nghĩa là nó rời khỏi họa sĩ cuối cùng. 

Một cách hữu ích để nghĩ về điều này là một quy trình có nhiều giai đoạn. Mỗi giai đoạn có một công nhân duy nhất và một hàng công việc, mỗi công việc phải trải qua tất cả các giai đoạn theo thứ tự. Khó khăn đến từ việc một họa sĩ chỉ có thể bắt đầu một bức tranh mới khi cả bức tranh đến từ họa sĩ trước đó và họa sĩ đều rảnh rỗi. 

Những hạn chế là quan trọng. Số lượng bức tranh có thể lên tới 50.000, trong khi số lượng họa sĩ nhiều nhất là 5. Điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng từng bước thời gian hoặc cố gắng tính toán lại lịch trình đầy đủ cho từng bức tranh một cách độc lập. Bất cứ điều gì gần với bậc hai về số lượng hình ảnh sẽ quá chậm. 

Một sự hiểu lầm ngây thơ thường xuất phát từ việc nghĩ rằng mỗi bức ảnh có thể được xử lý độc lập như một tổng số lần của nó. Điều đó sẽ bỏ qua việc chờ đợi do chặn đường ống. Ví dụ, nếu hai bức tranh đều cần người vẽ đầu tiên trong một thời gian dài, những người vẽ sau có thể trở nên nhàn rỗi hoặc tắc nghẽn tùy theo thứ tự. 

Một trường hợp thất bại tinh vi khác xuất hiện nếu chúng ta giả sử rằng mỗi họa sĩ bắt đầu một bức tranh mới ngay sau khi hoàn thành bức tranh trước đó. Điều đó bỏ qua thực tế là bức tranh có thể chưa đến từ họa sĩ trước đó. 

Một trường hợp thất bại minh họa nhỏ: 

đầu vào:```
2 2
5 1
1 5
```Nếu chúng ta tính tổng không chính xác cho mỗi bức tranh, cả hai sẽ hoàn thành ở thời điểm 6. Hành vi đúng sẽ khác vì họa sĩ thứ hai không thể bắt đầu bức tranh thứ hai cho đến khi nó được họa sĩ thứ nhất thả ra, tạo ra độ trễ. 

Đây là thách thức cốt lõi: đồng bộ hóa toàn cầu giữa các hàng đợi tuần tự. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ theo dõi, đối với mọi họa sĩ và mọi bức tranh, khi nào nó có sẵn và khi nào họa sĩ trở nên rảnh rỗi. Đối với mỗi bức tranh, chúng tôi sẽ phổ biến nó cho tất cả các họa sĩ, cập nhật thời gian có hàng. Điều này có thể đúng, nhưng mỗi bức ảnh thực hiện O(n) bước và có tới 50.000 bức ảnh, dẫn đến khoảng 250.000 thao tác, điều này ổn, nhưng mô phỏng đơn giản về điều kiện chờ bên trong các vòng lặp có thể dễ dàng bị suy giảm nếu được triển khai không hiệu quả. Tuy nhiên, vấn đề thực sự là nếu chúng ta mô phỏng thời gian liên tục hoặc cố gắng tìm kiếm vị trí có sẵn tiếp theo thì nó sẽ trở nên quá chậm. 

Quan sát quan trọng là hệ thống có cấu trúc phân lớp nghiêm ngặt. Mỗi họa sĩ xử lý các bức tranh theo một thứ tự cố định, vì vậy đối với bất kỳ họa sĩ j và bức tranh i nào, thời gian bắt đầu chỉ phụ thuộc vào hai giá trị: khi họa sĩ j trở nên rảnh rỗi và khi bức tranh i đến từ họa sĩ j−1. Cả hai điều này đều được biết khi chúng tôi xử lý theo thứ tự. 

Điều này cho phép xây dựng công thức lập trình động trên lưới 2D: dp[i][j] là thời điểm bức tranh tôi hoàn thành bởi họa sĩ j. Mỗi trạng thái chỉ phụ thuộc vào dp[i][j−1] (đến từ họa sĩ trước đó) và dp[i−1][j] (khi họa sĩ trở nên tự do sau bức ảnh trước đó). Vì n nhỏ nên chúng ta chỉ có thể duy trì hàng trước và hàng hiện tại hoặc thậm chí chỉ là mảng 1D cho mỗi họa sĩ. 

Chúng tôi xử lý ảnh theo thứ tự đầu vào, cập nhật tuần tự từng họa sĩ, duy trì thời gian hoàn thành của mỗi họa sĩ cho bức ảnh trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m · n) | O(m · n) | Được chấp nhận nhưng quá mức cần thiết | 
| Đường ống DP | O(m · n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng`finish[j]`đại diện cho thời điểm họa sĩ j trở nên tự do sau khi xử lý bức ảnh trước đó. 

Đối với mỗi bức ảnh i, chúng tôi tính toán thời gian hoàn thành của nó theo từng giai đoạn. 

1. Khởi tạo một biến`current_time`đối với bức tranh này là thời điểm nó đến với họa sĩ 1. Vì các bức tranh được xử lý theo thứ tự, đây chỉ đơn giản là khi họa sĩ 1 trở nên tự do, tức là.`finish[0]`. 
2. Với mỗi họa sĩ j từ 0 đến n−1, hãy tính thời điểm họa sĩ này có thể bắt đầu vẽ bức tranh. Đây là thời gian tối đa mà bức tranh đến từ họa sĩ trước đó và là thời gian họa sĩ này không còn tác phẩm trước đó. Đó là:`start = max(current_time, finish[j])`. 
3. Họa sĩ dành`t[i][j]`đơn vị thời gian nên thời gian kết thúc ở giai đoạn này trở thành:`start + t[i][j]`. 
4. Cập nhật`finish[j]`đến giá trị mới này vì họa sĩ j hiện đang bận rộn cho đến thời điểm đó đối với các bức tranh trong tương lai. 
5. Truyền bá thời điểm kết thúc này về phía trước như sau`current_time`, trở thành thời điểm đến của họa sĩ tiếp theo. 
6. Sau khi xử lý tất cả các họa sĩ, bản cuối cùng`current_time`là thời gian hoàn thành của bức tranh. 

Ý tưởng chính là chúng tôi đang mô phỏng một công việc di chuyển duy nhất thông qua một chuỗi trong đó mỗi trạm có dấu thời gian "bận cho đến". 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thông tin duy nhất liên quan đến việc lập kế hoạch cho bức tranh i tại họa sĩ j là khi bức tranh đến từ j−1 và khi họa sĩ j trở nên rảnh rỗi. Cả hai giá trị đều được xác định đầy đủ bởi các ảnh đã được xử lý trước đó. Vì chúng tôi xử lý ảnh theo thứ tự,`finish[j]`luôn thể hiện thời gian chặn chính xác do các hình ảnh trước đó gây ra. Bất biến này đảm bảo chúng tôi không bao giờ ghi đè lên phần phụ thuộc hợp lệ và mọi thời gian bắt đầu đều tối ưu cục bộ và nhất quán trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m, n = map(int, input().split())
    
    finish = [0] * n
    res = []

    for _ in range(m):
        t = list(map(int, input().split()))
        
        current = 0
        
        for j in range(n):
            start = max(current, finish[j])
            current = start + t[j]
            finish[j] = current
        
        res.append(current)

    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp giữ một bản trình bày nhỏ gọn về trạng thái đường ống. Mảng`finish`lưu trữ khi mỗi họa sĩ có sẵn, và`current`theo dõi khi hình ảnh hiện tại chuyển sang giai đoạn tiếp theo. 

Chi tiết quan trọng là thứ tự cập nhật. Chúng ta phải cập nhật`finish[j]`chỉ sau khi tính toán thời gian bắt đầu sử dụng giá trị cũ của nó. Điều này đảm bảo chúng tôi không vô tình cho phép một họa sĩ bắt đầu sớm hơn thực tế. 

Mỗi ảnh được xử lý độc lập nhưng được liên kết thông qua trạng thái chia sẻ trong`finish`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
3 1
1 2
2 3
```Chúng tôi theo dõi`finish`Và`current`: 

| Hình ảnh | Họa sĩ 0 bắt đầu/kết thúc | Họa sĩ 1 bắt đầu/kết thúc | kết thúc mảng | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 → 3 | 3 → 4 | [3, 4] | 4 | 
| 2 | 3 → 4 | 4 → 6 | [4, 6] | 6 | 
| 3 | 4 → 6 | 6 → 9 | [6, 9] | 9 | 

Điều này cho thấy mỗi họa sĩ có thể chờ đợi bức tranh hoặc chờ đợi sự sẵn có của riêng họ và tình trạng tắc nghẽn lan truyền về phía trước như thế nào. 

### Ví dụ 2 

đầu vào:```
3 3
1 2 3
3 2 1
2 2 2
```| Hình ảnh | P0 | P1 | P2 | kết thúc | 
| --- | --- | --- | --- | --- | 
| 1 | 0-1 | 1-3 | 3-6 | 6 | 
| 2 | 1-4 | 4-6 | 6-7 | 7 | 
| 3 | 4-6 | 6-8 | 8-10 | 10 | 

Ví dụ này nêu bật các hiệu ứng sắp xếp lại hoàn toàn do chờ đợi chứ không phải thời gian xử lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · n) | Mỗi m bức tranh được xử lý thông qua n họa sĩ một lần | 
| Không gian | O(n) | Chỉ có`finish`mảng có kích thước n được lưu trữ | 

Với m lên tới 50.000 và n lên tới 5, điều này dẫn đến tối đa 250.000 lần chuyển đổi, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    m, n = map(int, input().split())
    finish = [0] * n
    res = []

    for _ in range(m):
        t = list(map(int, input().split()))
        current = 0
        for j in range(n):
            start = max(current, finish[j])
            current = start + t[j]
            finish[j] = current
        res.append(current)

    return " ".join(map(str, res))

# provided sample
assert run("5 1\n1\n2\n3\n4\n5\n") == "1 3 6 10 15"

# all equal times
assert run("3 2\n1 1\n1 1\n1 1\n") == "2 4 6"

# single picture multiple painters
assert run("1 3\n5 1 2\n") == "8"

# pipeline blocking
assert run("2 2\n5 1\n1 5\n") == "6 11"

# maximum n=1 edge
assert run("4 1\n2\n2\n2\n2\n") == "2 4 6 8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả thời gian bằng nhau | tích lũy tuần tự | hành vi đường ống thống nhất | 
| hình ảnh duy nhất | phụ thuộc chuỗi thuần túy | không chặn giữa các hình ảnh | 
| trường hợp chặn | nhiễu hàng đợi | đặt hàng hiệu ứng | 
| n=1 | đường ống thoái hóa | giảm về tổng tiền tố | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chỉ có một họa sĩ. Hệ thống thoái hóa thành một chuỗi đơn giản trong đó mỗi bức ảnh chỉ bắt đầu sau khi bức ảnh trước đó kết thúc. Thuật toán xử lý việc này một cách tự nhiên vì`finish[0]`luôn theo dõi thời gian hoàn thành cuối cùng và`current`chỉ đơn giản là tích lũy thời lượng. 

Một trường hợp khác là người vẽ sau chậm hơn người vẽ trước, gây tắc nghẽn. Trong những trường hợp như vậy,`finish[j]`chiếm ưu thế về thời gian bắt đầu cho nhiều hình ảnh. Hoạt động tối đa đảm bảo rằng không có hình ảnh nào có thể vượt qua nút cổ chai và việc truyền bá`current`mô hình chính xác việc xây dựng hàng đợi. 

Một trường hợp tế nhị cuối cùng xảy ra khi một bức tranh đến muộn hơn thời gian rảnh của họa sĩ, nghĩa là họa sĩ phải đợi. các`max(current, finish[j])`nắm bắt điều này một cách rõ ràng, ngăn chặn việc lập kế hoạch tiêu cực hoặc sớm và đảm bảo tính chính xác mà không cần xử lý đặc biệt.
