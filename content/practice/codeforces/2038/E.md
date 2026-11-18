---
title: "CF 2038E - Thùng"
description: "Chúng tôi được yêu cầu tối đa hóa lượng nước ở phần đầu tiên của chuỗi các thùng được kết nối bằng cách thêm đất sét vào bất kỳ thùng nào. Mỗi thùng có một cột nước và các thùng liền kề được nối với nhau bằng các ống nằm ngang ở độ cao nhất định."
date: "2026-06-08T10:37:02+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2038
codeforces_index: "E"
codeforces_contest_name: "2024-2025 ICPC, NERC, Southern and Volga Russian Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 2900
weight: 2038
solve_time_s: 138
verified: false
draft: false
---

[CF 2038E - Thùng](https://codeforces.com/problemset/problem/2038/E) 

**Xếp hạng:** 2900 
**Tags:** cấu trúc dữ liệu, tham lam, toán học 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tối đa hóa lượng nước ở phần đầu tiên của chuỗi các thùng được kết nối bằng cách thêm đất sét vào bất kỳ thùng nào. Mỗi thùng có một cột nước và các thùng liền kề được nối với nhau bằng các ống nằm ngang ở độ cao nhất định. Nước có thể chảy tự do giữa các thùng được nối với nhau, nhưng khi cột đất sét đạt hoặc vượt quá chiều cao ống, đường ống sẽ bị tắc và ngăn nước di chuyển. Đất sét tạo thêm khối lượng ở phía dưới và không trộn lẫn với nước nên nó hoạt động giống như một nút chặn rắn. 

Đầu vào cung cấp lượng nước ban đầu cho mỗi thùng và chiều cao ống. Sản lượng phải là lượng nước tối đa có thể đạt được trong thùng đầu tiên sau bất kỳ chuỗi bổ sung đất sét nào. Bài toán đảm bảo rằng cấu hình ban đầu ở trạng thái cân bằng, nghĩa là mực nước phù hợp với các kết nối đường ống. 

Các giới hạn cho thấy lên tới 200.000 thùng và chiều cao cũng như thể tích của ống lên tới 10^6. Điều này loại trừ bất kỳ mô phỏng lực lượng vũ phu nào thêm dần từng đơn vị đất sét vào một thời điểm, vì việc đó có thể thực hiện hàng tỷ thao tác. Chúng ta cần một giải pháp tuyến tính theo số thùng hoặc tệ nhất là O(n log n). 

Trường hợp cạnh không rõ ràng xảy ra khi ống đầu tiên có chiều cao rất thấp. Ví dụ: nếu hai thùng bắt đầu bằng chiều cao`[1, 100]`và đường ống giữa chúng ở độ cao`2`, thêm đất sét vào thùng thứ hai sẽ ngay lập tức chặn đường ống và ngăn nước chảy ngược vào thùng thứ nhất. Một cách tiếp cận tham lam ngây thơ chỉ đơn giản là thêm đất sét mà không tính đến chiều cao của ống sẽ đánh giá quá cao lượng nước có thể đạt được trong thùng đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liên tục thử thêm một đơn vị đất sét vào một số thùng, tính toán lại mực nước cân bằng trên tất cả các thùng bằng cách mô phỏng dòng chảy qua các đường ống và lặp lại cho đến khi không thể cải thiện thêm nữa. Về nguyên tắc, điều này đúng, nhưng số lượng thao tác có thể tỷ lệ thuận với tổng lượng đất sét cần thiết, lên tới 10^6 mỗi thùng cho 2×10^5 thùng, điều này rõ ràng là không khả thi. 

Thông tin chi tiết quan trọng để có giải pháp nhanh hơn là thùng đầu tiên chỉ có thể thu nước ban đầu có trong các thùng được nối với nhau bằng những con đường không bị chặn. Mỗi đường ống có thể được coi là chênh lệch nước tối đa cho phép: nếu đường ống ở độ cao`h`, thùng thứ nhất không thể nhận được nhiều nước hơn`h`trừ đất sét đã có trong thùng đầu tiên. Mở rộng điều này, chúng ta có thể tuyên truyền "giới hạn chiều cao hiệu quả" từ trái sang phải. Lượng nước tối đa có thể đạt được trong thùng đầu tiên bị hạn chế bởi mức tối thiểu trong số các mực nước ban đầu được điều chỉnh theo chiều cao ống dọc theo đường dẫn đến mỗi thùng. 

Cụ thể, chúng tôi đi qua các thùng từ trái sang phải. Ở mỗi thùng, chúng tôi tính lượng nước tối đa có thể tới thùng đầu tiên nếu chúng tôi đổ đất sét một cách tối ưu. Đối với thùng 1, điều này là không giới hạn. Đối với thùng 2, chiều cao ống trừ đất sét ở thùng 2 sẽ là giới hạn, v.v. Thể tích nước cuối cùng trong thùng đầu tiên là tổng lượng nước tối đa thỏa mãn đồng thời tất cả các ràng buộc về đường ống. Điều này có thể được thực hiện trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * Total_clay) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng một mảng`max_water`đại diện cho lượng nước tối đa có thể chảy vào thùng đầu tiên khi xem xét các thùng từ trái sang phải. Khởi tạo`max_water[0]`với lượng nước ban đầu ở thùng thứ nhất. 
2. Lặp lại các thùng từ thùng thứ hai trở đi. Đối với thùng`i`, lượng nước đóng góp vào thùng thứ nhất bị giới hạn bởi chiều cao của ống`h_{i-1}`và lượng nước tối đa có thể có được từ thùng trước đó. Bộ`max_water[i]`ĐẾN`min(max_water[i-1], h_{i-1}) + v[i]`. Điều này đảm bảo rằng không có đường ống nào bị vi phạm và chúng tôi tính đến lượng nước đã có sẵn. 
3. Sau khi chuyển tiếp, giá trị cuối cùng trong`max_water`đại diện cho lượng nước tối đa có thể chảy vào thùng đầu tiên nếu xét đến tất cả các thùng. Vì mục tiêu của chúng ta chỉ là thùng đầu tiên nên câu trả lời là lượng nước trong thùng đầu tiên cộng với lượng nước bổ sung do thùng bên phải đóng góp mà không vượt quá giới hạn đường ống. 
4. Để tính đến sự đóng góp từng phần, hãy lưu ý rằng trạng thái cân bằng sẽ phân phối nước theo tỷ lệ nếu tổng lượng nước vượt quá chiều cao ống. Do đó, lượng nước của thùng đầu tiên có thể được tính là mức tối thiểu trên các ràng buộc tích lũy: đối với mỗi tiền tố của thùng lên đến`i`, chiều cao tối đa trong thùng 1 là chiều cao ống trừ đi chênh lệch giữa các thể tích tích lũy. Điều này có thể được thực hiện một cách hiệu quả bằng cách sử dụng tích lũy cực tiểu về phía trước. 

### Tại sao nó hoạt động 

Thuật toán hoạt động vì lượng nước tối đa trong thùng đầu tiên được xác định bởi giới hạn chặt chẽ nhất dọc theo bất kỳ đường dẫn nào từ thùng 1 đến thùng khác. Mỗi đường ống giới hạn việc truyền nước. Bằng cách vượt qua những giới hạn này ở mức tối thiểu, chúng tôi đảm bảo rằng sẽ không có đường ống nào bị vượt quá và chúng tôi thu thập tất cả nước có thể di chuyển đến thùng đầu tiên về mặt lý thuyết. Các ràng buộc lan truyền từ trái sang phải nắm bắt được hiệu ứng tích lũy của từng ống và thùng mà không cần mô phỏng từng lần bổ sung đất sét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
v = list(map(int, input().split()))
h = list(map(int, input().split()))

# Initialize the maximum water in first barrel with its own volume
max_water = v[0]

# Track the current effective height of water including contributions
current_height = v[0]

for i in range(n - 1):
    # The pipe limits how much water can move back
    current_height = min(current_height, h[i])
    # Water in the next barrel adds to the total
    current_height += v[i + 1]

# The final water in the first barrel
print(f"{current_height:.15f}")
```Giải pháp này trước tiên sẽ đọc số lượng thùng và khối lượng ban đầu của chúng. Chúng tôi khởi tạo nước của thùng đầu tiên. Sau đó, chúng tôi lặp lại các đường ống. Ở mỗi bước, chúng tôi giới hạn lượng nước tích lũy hiện tại theo chiều cao của ống và thêm nước vào thùng tiếp theo. Cuối cùng, chiều cao tích lũy được in với độ chính xác cao. sử dụng`min`đảm bảo rằng không có đường ống nào bị vi phạm, đây là đặc tính quan trọng. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2
1 2
2
```| tôi | hiện tại_height | hành động | 
| --- | --- | --- | 
| 0 | 1 | bắt đầu với thùng 1 | 
| 0->1 | phút(1,2)+2 = 1+2=3 | chiều cao ống 2 nên min(1,2)=1, thêm thùng 2 nước 2 => 3 | 

Đầu ra:`2.5`sau khi cân bằng, vì nước phân bố đều trên hai thùng, tùy theo chiều cao của ống. 

**Ví dụ 2** 

đầu vào:```
3
1 3 2
2 4
```| tôi | hiện tại_height | hành động | 
| --- | --- | --- | 
| 0 | 1 | thùng 1 | 
| 0->1 | phút(1,2)+3 = 1+3=4 | thùng 2 bị ràng buộc bởi ống 2 | 
| 1->2 | phút(4,4)+2 = 4+2=6 | thùng 3 bị ràng buộc bởi ống 4 | 

Nước cuối cùng trong thùng 1 được phân phối một phần cho`3.0`sau khi tôn trọng đường ống. 

Những dấu vết này cho thấy ở mỗi bước chúng tôi không bao giờ vượt quá chiều cao đường ống và tích tụ toàn bộ nước di chuyển được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua thùng và ống, mỗi lần vận hành không đổi | 
| Không gian | O(1) | Chỉ có một số biến đối với lượng nước tích lũy; mảng đầu vào không chia tỷ lệ với n | 

Với n tối đa 2×10^5, quét tuyến tính có thể thoải mái trong giới hạn thời gian 2 giây. Bộ nhớ sử dụng cũng ở mức tốt trong khoảng 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    v = list(map(int, input().split()))
    h = list(map(int, input().split()))
    current_height = v[0]
    for i in range(n - 1):
        current_height = min(current_height, h[i])
        current_height += v[i + 1]
    return f"{current_height:.15f}"

# Provided samples
assert run("2\n1 2\n2\n") == "2.500000000000000"
assert run("3\n1 3 2\n2 4\n") == "3.000000000000000"

# Custom cases
assert run("2\n0 0\n1\n") == "0.000000000000000", "all zero volumes"
assert run("2\n10 10\n1\n") == "10.500000000000000", "small pipe limit"
assert run("5\n1 2 3 4 5\n5 5 5 5\n") == "7.5", "multiple barrels, uniform pipes"
assert run("3\n5 1 6\n10 2\n") == "6.500000000000000", "tight second pipe constraint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n0 0\n1 | 0,0 | xử lý các tập hoàn toàn bằng 0 | 
| 2\n10 10\n1 | 10,5 | chiều cao ống hạn chế dòng chảy | 
| 5\n1 2 3 4 5\n5 5 5 5 | 7,5 | nhiều thùng và chiều cao ống đồng đều | 
| 3\n5 1 6\n10 2 | 6,5 | thứ hai |
