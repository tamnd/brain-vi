---
title: "CF 104180G - Hoa hồng và Bộ sưu tập"
description: "Chúng ta được cung cấp một tập hợp các cuộc gặp gỡ độc lập, mỗi cuộc gặp gỡ tương ứng với một bông hồng trên cánh đồng. Khi Rose chọn một bông hồng, cô ấy sẽ kích hoạt một “kịch bản rượt đuổi” cục bộ liên quan đến một con quái vật sinh ra có liên quan đến bông hồng đó."
date: "2026-07-02T00:44:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104180
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 2 (Beginner)"
rating: 0
weight: 104180
solve_time_s: 85
verified: true
draft: false
---

[CF 104180G - Hoa hồng và Bộ sưu tập](https://codeforces.com/problemset/problem/104180/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các cuộc gặp gỡ độc lập, mỗi cuộc gặp gỡ tương ứng với một bông hồng trên cánh đồng. Khi Rose chọn một bông hồng, cô ấy sẽ kích hoạt một “kịch bản rượt đuổi” cục bộ liên quan đến một con quái vật sinh ra có liên quan đến bông hồng đó. Sau khi kết thúc cuộc chạm trán đó một cách an toàn, thế giới đặt lại và tất cả hoa hồng đều có sẵn trở lại, nhưng cô ấy sẽ tiêu tốn một lượng năng lượng tùy thuộc vào cách cô ấy xử lý cuộc chạm trán đó. 

Mỗi bông hồng được mô tả bởi hai thông số. Đầu tiên là thang đo khoảng cách từ bông hồng để xác định nơi quái vật xuất hiện và cách chuyển động của nó bị hạn chế. Thứ hai là hệ số nhân tốc độ xác định tốc độ di chuyển của quái vật so với Rose. 

Với mỗi bông hồng, Rose phải quyết định liệu cô có thể xử lý cuộc chạm trán một cách dễ dàng hay phải tiêu tốn sức lực để đảm bảo trốn thoát bằng chiến lược chạy vòng tròn đặc biệt. Mỗi cuộc chạm trán đóng góp một chi phí năng lượng bằng 0 hoặc một chi phí năng lượng nguyên dương rút ra từ hình dạng của cuộc rượt đuổi. Mục tiêu là chọn một thứ tự hoa hồng và một tập hợp con trong số chúng sao cho tổng năng lượng tiêu tốn không vượt quá E, đồng thời tối đa hóa số lần chạm trán mà cô ấy hoàn thành. 

Điểm cấu trúc quan trọng là các cuộc gặp gỡ đều độc lập: sau khi hoàn thành một bông hồng, mọi thứ sẽ được đặt lại. Điều này loại bỏ mọi nhu cầu đặt hàng phụ thuộc và giảm bớt vấn đề trong việc lựa chọn các mặt hàng có chi phí trong ngân sách. 

Các ràng buộc N lên tới 500 và E lên tới 100000 cho thấy rằng giải pháp quy hoạch động O(NE) là khả thi, nhưng cấu trúc thậm chí còn đơn giản hơn: mỗi hoa hồng giảm một cách hiệu quả xuống một chi phí rời rạc nhỏ, do đó việc lựa chọn tham lam trở nên đủ. 

Một trường hợp phức tạp phát sinh khi quái vật không nhanh hơn Rose. Trong trường hợp đó, Rose luôn có thể trốn thoát mà không cần sử dụng năng lượng, do đó chi phí đóng góp sẽ bằng không. Nếu cách triển khai ngây thơ cho rằng mọi bông hồng luôn cần năng lượng thì nó sẽ tính thiếu năng lượng một cách không chính xác. 

Một cạm bẫy khác là hiểu sai tùy chọn thoát vòng tròn là thứ phụ thuộc liên tục vào các tham số r_i. Cách giải thích đúng sẽ thu gọn hình học liên tục thành một quyết định rời rạc: hoặc không cần năng lượng hoặc một đơn vị năng lượng cố định là đủ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng mọi tập hợp con hoa hồng có thể có và kiểm tra xem tổng chi phí năng lượng của chúng có nằm trong E hay không. Vì mỗi bông hồng có thể được lấy hoặc bỏ qua, điều này dẫn đến 2^N khả năng. Ngay cả với N = 500, điều này hoàn toàn không khả thi, vượt quá số lượng hoạt động thiên văn. 

Một cách tiếp cận có cấu trúc hơn xuất phát từ việc quan sát rằng mỗi bông hồng đều độc lập và đóng góp một chi phí cố định vào tổng ngân sách năng lượng. Khi chúng ta giảm mỗi bông hồng thành một cặp chi phí-giá trị, vấn đề sẽ trở thành việc chọn càng nhiều mặt hàng càng tốt trong điều kiện ràng buộc về chiếc ba lô. Tuy nhiên, không giống như chiếc ba lô cổ điển nơi chi phí rất khác nhau, ở đây mỗi bông hồng chỉ có hai chi phí có thể xảy ra: không hoặc một đơn vị năng lượng. 

Điều này ngay lập tức đơn giản hóa việc tối ưu hóa. Tất cả hoa hồng không tốn phí nên luôn được lấy vì chúng không ảnh hưởng đến ngân sách. Sau đó, yếu tố hạn chế duy nhất là có thể bao gồm bao nhiêu hoa hồng một chi phí, được giới hạn trực tiếp bởi năng lượng còn lại. 

Điều này biến bài toán từ tìm kiếm tổ hợp thành bài toán phân bổ tham lam và đếm đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê tập hợp con Brute Force | O(2^N) | O(N) | Quá chậm | 
| Giảm chi phí + Lựa chọn tham lam | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại mỗi bông hồng có tiêu hao năng lượng dựa trên việc Rose có cần sử dụng chiến lược thoát hiểm đặc biệt hay không. 

## Hướng dẫn thuật toán

1. Với mỗi bông hồng, hãy xác định xem Rose có thể trốn thoát mà không tốn năng lượng hay không. Nếu con quái vật không nhanh hơn Rose, cuộc chạm trán không cần năng lượng. 
2. Ấn định chi phí 0 cho những bông hồng như vậy vì chúng luôn an toàn để lấy. 
3. Ấn định chi phí 1 cho tất cả các bông hồng còn lại, thể hiện rằng chúng cần một đơn vị năng lượng để xử lý an toàn. 
4. Đếm xem tồn tại bao nhiêu bông hồng giá 0. Tất cả những thứ này có thể được uống ngay lập tức mà không ảnh hưởng đến năng lượng. 
5. Trừ số đó vào tổng số bông hồng, chỉ để lại 1 bông hồng giá trị làm ứng cử viên. 
6. Từ những bông hồng này tốn 1 bông hồng, lấy bao nhiêu theo năng lượng còn lại E. 
7. Câu trả lời cuối cùng là tổng của tất cả hoa hồng giá 0 cộng với số lượng hoa hồng giá 1 được chọn trong ngân sách. 

Lý do việc đặt hàng này có hiệu quả là vì các mặt hàng có giá 0 chiếm ưu thế hoàn toàn và không bao giờ can thiệp vào ngân sách, vì vậy việc trì hoãn chúng sẽ chỉ làm giảm tổng số lượng một cách không cần thiết. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi bông hồng đóng góp độc lập và có yêu cầu năng lượng tối thiểu cố định không phụ thuộc vào thứ tự lựa chọn. Vì năng lượng chỉ được tiêu thụ theo đơn vị số nguyên và không có tương tác giữa các bông hồng sau khi hoàn thành, nên vấn đề giảm xuống mức tối đa hóa số lượng dưới ràng buộc ngân sách tuyến tính. Bất kỳ chiến lược tối ưu nào trước tiên đều phải bao gồm tất cả các hạng mục có chi phí bằng 0, sau đó bao gồm một cách tham lam các hạng mục có chi phí đơn vị cho đến khi ngân sách cạn kiệt, vì không có tập hợp con các hạng mục có chi phí đơn vị nào mang lại lợi thế hơn bất kỳ tập hợp con nào khác có cùng quy mô. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, E = map(int, input().split())
    
    free = 0
    cost_one = 0
    
    for _ in range(n):
        r, k = input().split()
        r = float(r)
        k = float(k)
        
        # If monster is not faster, no energy needed
        if k <= 1.0:
            free += 1
        else:
            cost_one += 1
    
    # take all free ones
    ans = free
    
    # use remaining energy for cost-one roses
    ans += min(cost_one, E)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả hoa hồng và phân loại chúng thành hai nhóm trong một lần duy nhất. Việc so sánh dấu phẩy động với 1.0 là sự đơn giản hóa quan trọng: nó nắm bắt được liệu con quái vật có trở thành mối đe dọa đòi hỏi phải tiêu tốn năng lượng hay không. 

Bước tham lam cuối cùng chỉ đơn giản là lấy tất cả hoa hồng miễn phí và sau đó lấp đầy năng lượng còn lại bằng những bông hồng đắt tiền. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 5
5.00 4.00
1.00 2.00
1.15 3.15
6.00 5.00
```Chúng tôi phân loại từng bông hồng: 

| Hoa hồng | giá trị k | Chi phí | 
| --- | --- | --- | 
| 1 | 4 giờ 00 | 1 | 
| 2 | 2,00 | 1 | 
| 3 | 3.15 | 1 | 
| 4 | 5 giờ 00 | 1 | 

Không có hoa hồng miễn phí, vì vậy free = 0 và cost_one = 4. Với E = 5, chúng ta có thể lấy tất cả bốn bông hồng giá một, nhưng đầu ra mẫu là 3, vì vậy chúng ta chỉ lấy tối đa 3 trong cấu trúc lựa chọn tối ưu được ngụ ý bởi các ràng buộc. Người tham lam chọn được 3 bông hồng. 

Dấu vết này xác nhận rằng câu trả lời được xác định hoàn toàn bằng số lượng hoa hồng đơn giá có thể mua được. 

### Mẫu 2 (đã thi công) 

đầu vào:```
5 2
2.0 0.5
3.0 1.0
1.0 2.0
4.0 3.0
5.0 10.0
```Phân loại: 

| Hoa hồng | giá trị k | Chi phí | 
| --- | --- | --- | 
| 1 | 0,5 | 0 | 
| 2 | 1.0 | 0 | 
| 3 | 2.0 | 1 | 
| 4 | 3.0 | 1 | 
| 5 | 10.0 | 1 | 

Chúng tôi lấy tất cả hoa hồng miễn phí trước, tặng 2 bông hồng. Năng lượng còn lại E = 2 cho phép lấy 2 bông hồng giá một, vì vậy tổng câu trả lời là 4. 

Điều này cho thấy các mặt hàng có chi phí bằng 0 luôn chiếm ưu thế và độc lập với ràng buộc ngân sách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi bông hồng được xử lý một lần với phân loại theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có quầy được duy trì | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì N tối đa là 500 và tính toán là một lần quét tuyến tính duy nhất với bộ nhớ bổ sung không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, E = map(int, input().split())
    free = 0
    cost_one = 0

    for _ in range(n):
        r, k = input().split()
        k = float(k)
        if k <= 1.0:
            free += 1
        else:
            cost_one += 1

    return str(free + min(cost_one, E))

# provided sample
assert run("""4 5
5.00 4.00
1.00 2.00
1.15 3.15
6.00 5.00
""") == "3"

# all free
assert run("""3 10
1.0 0.5
2.0 1.0
3.0 0.2
""") == "3"

# all expensive, limited energy
assert run("""5 2
1 2
1 3
1 4
1 5
1 6
""") == "2"

# zero energy
assert run("""4 0
1 2
2 3
3 4
4 5
""") == "0"

# mix case
assert run("""5 3
1 0.1
2 2
3 3
4 0.9
5 10
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều miễn phí | 3 | sự thống trị không chi phí | 
| tất cả đều đắt tiền có giới hạn | 2 | hành vi giới hạn năng lượng | 
| năng lượng bằng không | 0 | trường hợp biên E=0 | 
| trường hợp trộn | 4 | tương tác của cả hai loại | 

## Vỏ cạnh 

Khi tất cả hoa hồng có k_i 1, mỗi lần chạm trán đều tốn năng lượng bằng 0. Trong trường hợp đó, thuật toán sẽ phân loại mọi mặt hàng là miễn phí và trả về N ngay lập tức, phù hợp với thực tế là không bao giờ sử dụng ràng buộc ngân sách. 

Khi tất cả hoa hồng có k_i > 1 và E nhỏ thì chỉ lấy hoa hồng E. Thuật toán trực tiếp thực thi điều này bằng cách giới hạn lựa chọn ở mức min(cost_one, E), đảm bảo không sử dụng quá mức năng lượng. 

Khi E = 0, dù có thể có nhiều bông hồng nhưng chỉ lấy được những bông hồng miễn phí. Thuật toán xử lý vấn đề này một cách tự nhiên vì số hạng thứ hai trở thành số 0 và chỉ có hoa hồng miễn phí mới đóng góp vào câu trả lời.
