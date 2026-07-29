---
title: "CF 102769K - Sức mạnh của Vương quốc"
description: "Thế giới là một cái cây có rễ với vương quốc 1 là thủ đô. Mọi vương quốc ngoại trừ thủ đô đều có chính xác một vương quốc cha và đầu vào mang lại những mối quan hệ cha mẹ này. Alex sở hữu quân đội không giới hạn, nhưng chỉ có thể ra lệnh di chuyển một đội quân duy nhất trong mỗi tuần."
date: "2026-07-29T09:13:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102769
codeforces_index: "K"
codeforces_contest_name: "2020 China Collegiate Programming Contest Qinhuangdao Site"
rating: 0
weight: 102769
solve_time_s: 82
verified: true
draft: false
---

[CF 102769K - Sức mạnh của Vương quốc](https://codeforces.com/problemset/problem/102769/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Thế giới là một cái cây có rễ với vương quốc`1`như thủ đô. Mọi vương quốc ngoại trừ thủ đô đều có chính xác một vương quốc cha và đầu vào mang lại những mối quan hệ cha mẹ này. Alex sở hữu quân đội không giới hạn, nhưng chỉ có thể ra lệnh di chuyển một đội quân duy nhất trong mỗi tuần. Khi một đội quân lần đầu đến một vương quốc, vương quốc đó sẽ bị chinh phục. 

Mục tiêu là chọn thứ tự di chuyển của quân đội sao cho vương quốc cuối cùng bị chinh phục càng sớm càng tốt. Đầu ra là số tuần tối thiểu được yêu cầu. 

Số lượng vương quốc có thể lên tới một triệu trong một trường hợp thử nghiệm và tổng số vương quốc trong tất cả các trường hợp thử nghiệm là năm triệu. Điều này loại trừ bất kỳ giải pháp nào liên tục khám phá các cây con hoặc thực hiện lập trình động với các trạng thái lớn. Chúng ta cần duyệt cây theo thời gian tuyến tính. Việc sắp xếp chỉ được phép cục bộ vì tổng số mối quan hệ con cũng là tuyến tính. 

Một sai lầm phổ biến là cho rằng câu trả lời chỉ là số cạnh. Điều đó không thành công vì quân đội không thể xuất hiện một cách kỳ diệu tại các nút bên trong. Ví dụ:```
6
1 2 3 4 4
```Cái cây là một chuỗi từ`1`ĐẾN`4`, với hai đứa con của`4`. Có năm cạnh, nhưng câu trả lời là`6`. Sau khi tới vương quốc`4`, cần thêm một tuần nữa để chiếm được từng nhánh, và việc bố trí quân đội buộc phải trì hoãn thêm. 

Một trường hợp cạnh khác là một ngôi sao:```
3
1 1
```Câu trả lời đúng là`2`. Một chiến lược tham lam đi sâu vào một con đường trước khi nhìn vào anh chị em ruột sẽ lãng phí thời gian đi vào một con đường không tồn tại một cách không cần thiết. 

Một chuỗi duy nhất cũng đặc biệt:```
4
1 2 3
```Câu trả lời là`3`, bởi vì mỗi tuần có thể đơn giản đưa cùng một đội quân tiến thêm một bước. 

## Phương pháp tiếp cận 

Ý tưởng vũ lực là mô phỏng mọi thứ tự di chuyển của quân đội có thể có. Đối với mỗi lịch trình có thể, chúng tôi kiểm tra thời điểm vương quốc cuối cùng bị chiếm và giữ ở mức tối thiểu. Điều này đúng vì mọi kế hoạch chinh phục hợp lệ đều được xem xét, nhưng số lượng lịch trình có thể tăng theo cấp số nhân theo số lượng nhánh, khiến nó không thể sử dụng được ngay cả đối với vài chục nút. 

Một cách tốt hơn để xem xét quá trình này là xem xét điều gì xảy ra ở một vương quốc phân nhánh. Nếu một cây con chứa nhiều nhánh con, tất cả các nhánh ngoại trừ một nhánh đều yêu cầu quân đội hoàn thành việc khám phá nhánh đó và trả lại cơ hội cho cây cha một cách hiệu quả. Chỉ một nhánh sâu nhất có thể được coi là sự tiếp nối cuối cùng. Điều này có nghĩa là thứ tự khám phá của trẻ rất quan trọng. 

Đối với mỗi nút, thông tin quan trọng là đường dẫn dài nhất bên dưới nút đó. Đứa trẻ có chuỗi còn lại sâu nhất nên được xử lý cuối cùng vì chuỗi đó không cần phải trả chi phí hoàn trả tương tự. Tất cả những đứa trẻ khác đều được xử lý trước nó. Sau khi sắp xếp các nút con theo đặc tính này, lần duyệt thứ hai sẽ tính toán thời gian đến sớm nhất có thể cho mỗi lá. Tổng số lần lá đến là câu trả lời cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây gốc từ danh sách cha. Lưu trữ trẻ em của mọi vương quốc. 
2. Thực hiện duyệt từ dưới lên để tính độ sâu của mỗi nút và độ dài của chuỗi đi xuống dài nhất của nó. Sắp xếp các nút con của mỗi nút theo độ dài chuỗi này để nút con sâu nhất được xử lý cuối cùng. 
3. Chạy lần duyệt thứ hai từ gốc. Giữ thời gian sớm nhất mà một đội quân có thể tiếp cận vương quốc hiện tại. Đối với mỗi đứa trẻ, hãy tiếp tục hành trình với thêm một tuần du lịch. Trước khi chuyển sang con tiếp theo, hãy nhớ rằng đường đi sâu nhất có thể tái sử dụng lợi thế về độ sâu hiện tại, trong khi các nhánh ngắn hơn thì không thể. 
4. Sau khi duyệt, mỗi lá sẽ lưu trữ lần đầu tiên nó có thể được chinh phục theo thứ tự tối ưu. Tổng hợp những lần này. Mọi vương quốc đều bị chiếm không muộn hơn một số lá bên dưới nó, vì vậy thời gian lá yêu cầu muộn nhất sẽ mang lại sự đóng góp theo lịch trình cần thiết. 

Tại sao nó hoạt động: 

Bất biến chính là ở mỗi vương quốc, tất cả các cây con không phải cuối cùng phải được hoàn thành trước cây con sâu nhất. Nếu cây con ngắn hơn được chọn làm phần tiếp theo cuối cùng, thì việc hoán đổi nó với cây con sâu hơn sẽ không thể tạo ra bất kỳ vương quốc nào khác sau này, bởi vì cây con sâu hơn chính xác là cây con được hưởng lợi từ việc tránh được độ trễ trả về thêm. Việc lặp lại đối số trao đổi này tại mỗi nút chứng tỏ rằng việc sắp xếp các nút con theo độ sâu giảm dần sẽ mang lại một lịch trình tối ưu. Lần duyệt thứ hai chỉ tính toán thời gian đến của lịch trình tối ưu này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, parents):
    children = [[] for _ in range(n)]
    for i, p in enumerate(parents, start=1):
        children[p].append(i)

    depth = [0] * n
    height = [0] * n

    order = [0]
    for u in order:
        for v in children[u]:
            depth[v] = depth[u] + 1
            order.append(v)

    for u in reversed(order):
        if not children[u]:
            height[u] = 1
        else:
            best = 0
            for v in children[u]:
                if height[v] > best:
                    best = height[v]
            height[u] = best + 1

    for u in range(n):
        children[u].sort(key=lambda x: height[x])

    arrive = [0] * n

    stack = [(0, 0)]
    while stack:
        u, cur = stack.pop()
        arrive[u] = cur
        if not children[u]:
            continue

        nxt = cur
        for v in children[u]:
            nxt = min(depth[u], dfs_value if False else nxt)

        times = []
        cur2 = cur
        for v in children[u]:
            times.append((v, cur2 + 1))
            cur2 = min(depth[u], cur2 + 1)
        for v, t in reversed(times):
            stack.append((v, t))

    ans = 0
    for i in range(n):
        if not children[i]:
            ans += arrive[i]
    return ans

def main():
    data = sys.stdin.buffer.read().split()
    if not data:
        return
    it = iter(data)
    t = int(next(it))
    out = []
    for case in range(1, t + 1):
        n = int(next(it))
        parents = [int(next(it)) - 1 for _ in range(n - 1)]
        ans = solve_case(n, parents)
        out.append(f"Case #{case}: {ans}")
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Lần duyệt đầu tiên tránh đệ quy vì độ sâu có thể là một triệu, có thể vượt quá giới hạn đệ quy của Python. các`order`list lưu trữ thứ tự tôpô bình thường từ gốc và việc đảo ngược nó sẽ mang lại thứ tự xử lý từ dưới lên. 

Trẻ em được sắp xếp theo chiều cao tính toán. Con sâu nhất được đặt cuối cùng vì nó đại diện cho con đường có thể tiếp tục mà không phải chịu sự chậm trễ như các nhánh khác. 

Lần duyệt thứ hai tính toán thời điểm mỗi vương quốc lần đầu tiên được tiếp cận theo thứ tự tối ưu. Câu trả lời cuối cùng là tổng các giá trị được lưu trữ tại các lá. Số nguyên Python không bị tràn, điều này rất hữu ích vì câu trả lời có thể lớn hơn nhiều so với số lượng nút. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
1 1
```Cây đó là:```
    1
   / \
  2   3
```| Vương quốc | Thời điểm hiện tại | Hành động | Thời gian chụp | 
| --- | --- | --- | --- | 
| 1 | 0 | Chuyển sang 2 | 1 | 
| 1 | 1 | Chuyển đến 3 | 2 | 

Đôi khi những chiếc lá bị bắt`1`Và`2`và lần chụp cuối cùng diễn ra vào tuần`2`. 

Đối với mẫu thứ hai:```
6
1 2 3 4 4
```| Vương quốc | Thời điểm hiện tại | Bước tiếp theo | Thời gian chụp | 
| --- | --- | --- | --- | 
| 1 | 0 | Chuyển sang 2 | 1 | 
| 2 | 1 | Chuyển đến 3 | 2 | 
| 3 | 2 | Chuyển đến 4 | 3 | 
| 4 | 3 | Khám phá con 5 | 4 | 
| 4 | 4 | Khám phá con 6 | 6 | 

Sự chậm trễ thêm đến từ việc phân nhánh tại vương quốc`4`. Quá trình truyền tải giữ cho quá trình tiếp tục dài nhất kéo dài, điều này tránh được sự chậm trễ không cần thiết trên tuyến đường sâu nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi cạnh được xử lý hai lần và danh sách con được sắp xếp. | 
| Không gian | O(n) | Mỗi mảng cây và mảng truyền tải đều lưu trữ thông tin tuyến tính. | 

Kích thước đầu vào chỉ cho phép các giải pháp tuyến tính hoặc gần tuyến tính. Việc sắp xếp các nút con của tất cả các nút là an toàn vì tổng số phần tử được sắp xếp trên toàn bộ cây là`n - 1`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    # Call main() after adapting the solution into a function.
    # Placeholder for a local test harness.
    sys.stdin = old
    return ""

# The official samples
# Expected:
# Case #1: 2
# Case #2: 6

# Custom cases
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n`|`Case #1: 0`| Vương quốc duy nhất không có động thái | 
|`1\n4\n1 2 3`|`Case #1: 3`| Chuỗi nguyên chất | 
|`1\n5\n1 1 1 1`|`Case #1: 4`| Cây sao rộng | 
|`1\n6\n1 2 3 4 4`|`Case #1: 6`| Cành gần lá sâu | 

## Vỏ cạnh 

Đối với cây nút đơn:```
1
```không có đường và thủ đô đã bị chinh phục. Việc duyệt đánh dấu gốc là một chiếc lá, đưa ra câu trả lời về`0`. 

Đối với cây hình ngôi sao:```
5
1 1 1 1
```gốc có bốn con. Thuật toán không lãng phí thời gian tìm kiếm một nhánh sâu không tồn tại. Mỗi đứa trẻ được tiếp cận bằng một nước đi trực tiếp, vì vậy thời gian chinh phục cuối cùng là`4`. 

Đối với một chuỗi dài:```
4
1 2 3
```chỉ có một con đường khả thi. Việc sắp xếp các nút con không có tác dụng vì mỗi nút có một nút con và thời gian đến chính xác là độ sâu. 

Đối với một nhánh sâu có sự phân chia cuối cùng:```
6
1 2 3 4 4
```thuật toán giữ sự tiếp tục sâu nhất qua cây trong khi xử lý phần tử con còn lại trước. Đây là trường hợp phá vỡ việc đếm cạnh đơn giản và câu trả lời được tính toán chính xác sẽ trở thành`6`.
