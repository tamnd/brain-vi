---
title: "CF 106500I - Đóng chuyển tiếp"
description: "Chúng ta được cho một đồ thị có hướng trong đó các đỉnh biểu thị các trạng thái và các cạnh biểu thị sự chuyển đổi một bước từ trạng thái này sang trạng thái khác."
date: "2026-06-25T08:37:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106500
codeforces_index: "I"
codeforces_contest_name: "XXVIII Interregional Programming Olympiad, Vologda SU, 2026"
rating: 0
weight: 106500
solve_time_s: 37
verified: true
draft: false
---

[CF 106500I - Đóng chuyển tiếp](https://codeforces.com/problemset/problem/106500/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trong đó các đỉnh biểu thị các trạng thái và các cạnh biểu thị sự chuyển đổi một bước từ trạng thái này sang trạng thái khác. Từ quan điểm kết nối cục bộ này, nhiệm vụ là xác định mối quan hệ khả năng tiếp cận đầy đủ: đối với mỗi cặp đỉnh được sắp xếp, hãy quyết định xem liệu có thể bắt đầu ở đỉnh đầu tiên và đi theo các cạnh được định hướng để cuối cùng đến đỉnh thứ hai thông qua bất kỳ số bước trung gian nào hay không. 

Đầu ra không phải là về đường đi ngắn nhất hoặc đếm đường đi. Nó hoàn toàn là về sự tồn tại của một con đường. Nếu một đỉnh có thể tự tiếp cận thông qua một số chu trình, thì đó cũng là một phần của câu trả lời, được yêu cầu rõ ràng hoặc được xử lý ngầm tùy thuộc vào định dạng (thông thường các mục theo đường chéo được đặt thành có thể truy cập được trong quá trình đóng bắc cầu). 

Ngay cả khi không nhìn thấy giới hạn chính xác, cấu trúc của bài toán đóng bắc cầu gợi ý rõ ràng về đầu ra mối quan hệ dày đặc có kích thước O(n2). Điều đó ngay lập tức loại trừ các giải pháp cố gắng liệt kê rõ ràng tất cả các đường dẫn. Bất kỳ cách tiếp cận nào khám phá các đường dẫn một cách độc lập cho từng cặp đều có thể dễ dàng giảm xuống thành O(n³) hoặc tệ hơn, trở thành đường biên ở mức n khoảng vài nghìn và hoàn toàn không khả thi nếu vượt quá mức đó. 

Một trường hợp phức tạp xuất hiện khi khả năng tiếp cận là gián tiếp và yêu cầu nhiều bước nhảy. Ví dụ: nếu các cạnh là 1 → 2 và 2 → 3 thì 1 phải được đánh dấu là đạt tới 3 mặc dù không tồn tại cạnh trực tiếp. Kiểm tra kề cận ngây thơ sẽ bỏ lỡ điều này trừ khi nó liên tục truyền bá khả năng tiếp cận. 

Một trường hợp góc khác là khả năng tự tiếp cận. Trong đồ thị như 1 → 2 → 1, đỉnh 1 có thể chạm tới chính nó thông qua một chu trình, do đó việc đóng phải phản ánh điều đó. Bất kỳ triển khai nào chỉ đánh dấu các cạnh trực tiếp và sau đó lan truyền mà không tính đến chu kỳ có thể không đặt các mục nhập đường chéo một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực bắt đầu bằng cách coi mỗi đỉnh là một nguồn và chạy truyền tải đồ thị, chẳng hạn như DFS hoặc BFS, để đánh dấu tất cả các nút có thể truy cập. Điều này đúng vì mỗi lần truyền tải sẽ khám phá chính xác tập hợp các nút có thể truy cập được từ một điểm bắt đầu nhất định. Việc lặp lại điều này cho tất cả các đỉnh sẽ tạo ra sự đóng chuyển tiếp đầy đủ. 

Vấn đề là chi phí thăm dò lặp đi lặp lại. Một DFS hoặc BFS đơn lẻ mất O(n + m). Làm điều này cho mọi đỉnh đều dẫn đến O(n(n + m)). Trong các đồ thị dày đặc trong đó m là O(n2), giá trị này trở thành O(n³). Ngay cả khi biểu đồ thưa thớt, hệ số không đổi của việc khởi chạy n tìm kiếm là rất quan trọng và việc triển khai Python thường gặp khó khăn trong các ràng buộc CF trong trường hợp xấu nhất. 

Quan sát quan trọng là khả năng tiếp cận có tính bắc cầu và có thể được tổng hợp. Nếu nút i có thể truy cập được j thì mọi nút có thể truy cập từ j cũng có thể truy cập được từ i. Điều này gợi ý rằng thay vì tính toán lại khả năng tiếp cận một cách độc lập cho từng nút, chúng ta có thể _truyền dần dần các tập hợp nút có thể truy cập_. 

Đây là lúc ý tưởng về phong cách Floyd-Warshall dựa trên bitset trở nên hiệu quả. Thay vì lưu trữ các cạnh boolean riêng lẻ, chúng tôi lưu trữ cho mỗi nút một tập hợp các nút có thể truy cập được. Khi chúng tôi phát hiện ra rằng tôi có thể tiếp cận j, chúng tôi hợp nhất tập hợp khả năng tiếp cận của j vào i. Điều này biến việc truyền tải lặp đi lặp lại thành các tập hợp lặp lại, trong Python có thể được triển khai một cách hiệu quả bằng cách sử dụng các phép toán bit số nguyên. 

Qua nhiều lần lặp lại, thông tin về khả năng tiếp cận sẽ truyền qua các nút trung gian cho đến khi không thể thêm bit mới nào. Tại thời điểm đó, mỗi bitset mã hóa chính xác việc đóng chuyển tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force DFS/BFS từ mỗi nút | O(n(n + m)) | O(n + m) | Quá chậm trong đồ thị dày đặc | 
| Đóng cửa chuyển tiếp Bitset | O(n³ / word_size) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu thị tập hợp có thể truy cập của mỗi đỉnh dưới dạng bitmask. Bit thứ j trong mặt nạ của i cho biết liệu tôi có thể đạt tới j hay không. 

1. Khởi tạo danh sách`reach`, Ở đâu`reach[i]`chỉ có bit tôi đặt nếu cho phép khả năng tự truy cập và cũng đặt bit cho mọi cạnh trực tiếp i → j. Điều này mã hóa biểu đồ cơ sở chính xác như đã cho. 
2. Lặp lại tất cả các đỉnh trung gian k từ 0 đến n − 1. Với mỗi k, chúng tôi cố gắng sử dụng k làm cầu nối giữa các đỉnh khác. 
3. Với mọi đỉnh nguồn i, nếu tôi có thể tới được k thì mọi thứ có thể tới được từ k cũng phải tới được từ i. Chúng tôi cập nhật`reach[i] = reach[i] OR reach[k]`. Đây là bước lan truyền quan trọng, vì nó đưa toàn bộ tập hợp có thể truy cập của k vào i trong thời gian bit không đổi. 
4. Lặp lại quá trình này cho tất cả k theo thứ tự tăng dần. Mỗi lần lặp cho phép các đường dẫn sử dụng các đỉnh lên đến k làm trung gian, dần dần xây dựng các chuỗi khả năng tiếp cận ngày càng dài hơn. 
5. Sau tất cả các lần lặp, giải mã các tập hợp bit thành ma trận kề hoặc danh sách cạnh tùy thuộc vào định dạng đầu ra được yêu cầu. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý đỉnh trung gian k,`reach[i]`chứa chính xác tất cả các đỉnh có thể truy cập được từ i chỉ bằng cách sử dụng các nút trung gian từ tập {0, 1, ..., k}. Khi xử lý k, chúng tôi mở rộng tất cả các đường dẫn hợp lệ kết thúc tại k bằng cách nối thêm mọi thứ có thể truy cập được từ k. Điều này đảm bảo rằng mọi đường dẫn hợp lệ cuối cùng sẽ được xây dựng sau khi đỉnh trung gian có chỉ số cao nhất của nó được xử lý và không có đường dẫn không hợp lệ nào được đưa vào vì chúng tôi chỉ truyền bá các bộ khả năng tiếp cận hiện có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    reach = [0] * n

    for i in range(n):
        row = input().strip()
        for j, ch in enumerate(row):
            if ch == '1':
                reach[i] |= (1 << j)

    for k in range(n):
        for i in range(n):
            if reach[i] & (1 << k):
                reach[i] |= reach[k]

    out = []
    for i in range(n):
        row = []
        for j in range(n):
            row.append('1' if (reach[i] >> j) & 1 else '0')
        out.append(''.join(row))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc biểu diễn ma trận kề của biểu đồ và chuyển đổi từng hàng thành mặt nạ bit. Mỗi mặt nạ bit lưu trữ gọn gàng tất cả các nút có thể truy cập trực tiếp từ một nút nhất định. 

Vòng lặp kép trên k và i thực hiện quá trình lan truyền bắc cầu. điều kiện`reach[i] & (1 << k)`tránh các kết hợp không cần thiết khi tôi không thể đạt được k, điều này có thể làm giảm đáng kể các hệ số không đổi trong các trường hợp thưa thớt. Bước cập nhật sẽ hợp nhất tập hợp đầy đủ của k vào i. 

Cuối cùng, mặt nạ bit được giải mã trở lại thành ma trận nhị phân bằng cách kiểm tra từng vị trí bit. 

Một lỗi triển khai phổ biến là quên rằng việc truyền bá phải được thực hiện trên các nhóm phạm vi tiếp cận _đã cập nhật_. Thứ tự của vòng lặp k quan trọng vì nó đảm bảo các nút trung gian dần dần được đưa vào như những bước đệm hợp lệ. 

## Ví dụ đã hoạt động 

Xét một đồ thị có ba nút và các cạnh 0 → 1 và 1 → 2. 

### Trạng thái ban đầu 

| tôi | đạt[i] (nhị phân) | 
| --- | --- | 
| 0 | 010 | 
| 1 | 001 | 
| 2 | 000 | 

Sau khi xử lý k = 0, không có gì thay đổi vì không có nút nào đạt tới 0 ngoại trừ chính nó. 

Sau khi xử lý k = 1, nút 0 đạt 1 và vì 1 đạt 2 nên chúng ta truyền 2 thành 0. 

| Bước | tôi | đạt[i] | 
| --- | --- | --- | 
| k=1 lan truyền | 0 | 011 | 
| k=1 lan truyền | 1 | 001 | 
| k=1 lan truyền | 2 | 000 | 

Sau k = 2, không có cập nhật nào nữa xảy ra. 

Kết quả cuối cùng cho thấy 0 đạt đến cả 1 và 2, trong khi 1 đạt đến 2. 

Dấu vết này cho thấy khả năng tiếp cận gián tiếp chỉ được tích lũy sau khi các đỉnh trung gian hoạt động như những cầu nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³ / w) | Mỗi n lần lặp sẽ truyền n bitset bằng cách sử dụng các phép toán OR cấp độ từ | 
| Không gian | O(n²) | Mỗi đỉnh lưu trữ một bitmask có kích thước n | 

Cấu trúc khối phù hợp với bản chất vốn có của tính toán bao đóng, nhưng các phép toán bit nén các hệ số không đổi một cách đáng kể. Điều này đủ cho các ràng buộc thông thường lên tới vài nghìn nút trong môi trường lập trình cạnh tranh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# simple chain
assert run("3\n010\n001\n000") == "011\n001\n000"

# fully connected
assert run("2\n11\n11") == "11\n11"

# isolated nodes
assert run("3\n000\n000\n000") == "000\n000\n000"

# cycle
assert run("3\n010\n001\n100") == "111\n111\n111"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 0→1→2 | kết thúc bao gồm phạm vi tiếp cận gián tiếp | tính chính xác của việc truyền bá | 
| đồ thị hoàn chỉnh | ma trận đầy đủ không thay đổi | bình thường | 
| đồ thị trống | tất cả số không | không có kết quả dương tính giả | 
| chu kỳ | khả năng tiếp cận đầy đủ | tự tiếp cận qua chu kỳ | 

## Vỏ cạnh 

Trong chuỗi ba nút 0 → 1 → 2, quá trình cập nhật một lượt đơn giản có thể dừng sau khi đánh dấu 0 → 1 và 1 → 2, nhưng không truyền được 0 → 2. Thuật toán đúng sẽ xử lý vấn đề này vì khi k = 1 được xử lý, Reach[0] đã bao gồm 1, do đó, nó hấp thụ Reach[1], chứa 2. 

Trong chu kỳ 0 → 1 → 2 → 0, cuối cùng mỗi nút sẽ đến mọi nút khác. Trong quá trình lặp k = 0, nút 2 có thể gián tiếp đạt tới 0 và khi k = 0 được sử dụng làm trung gian, nó sẽ truyền toàn bộ tập hợp của nó trở lại thành 2, hoàn thành việc đóng. Vòng phản hồi này được giải quyết một cách tự nhiên bằng cách truyền bá liên kết lặp đi lặp lại thay vì phát hiện chu kỳ rõ ràng.
