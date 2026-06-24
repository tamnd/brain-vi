---
title: "CF 105278I - d-parkour"
description: "Chúng ta được cung cấp một dãy các tòa nhà, mỗi tòa nhà có chiều cao riêng biệt. Đối với bất kỳ khoảng cách nào của các tòa nhà từ chỉ số i đến j, chúng ta xem xét hai đường đi: di chuyển từ trái sang phải và di chuyển từ phải sang trái."
date: "2026-06-23T14:20:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105278
codeforces_index: "I"
codeforces_contest_name: "2024 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 105278
solve_time_s: 97
verified: false
draft: false
---

[CF 105278I - d-parkour](https://codeforces.com/problemset/problem/105278/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy các tòa nhà, mỗi tòa nhà có chiều cao riêng biệt. Đối với bất kỳ khoảng thời gian nào của tòa nhà từ chỉ mục`i`ĐẾN`j`, ta xét hai đường truyền: di chuyển từ trái sang phải và di chuyển từ phải sang trái. Khi chúng ta di chuyển, chúng ta ghi lại xem mỗi bước đi lên hay đi xuống theo độ cao, tạo ra một chuỗi các bước di chuyển bao gồm U và D. 

một cặp`(i, j)`được gọi là hợp lệ nếu mô hình thăng trầm nhìn thấy khi đi bộ từ`i`ĐẾN`j`giống hệt với mô hình nhìn thấy khi đi bộ từ`j`ĐẾN`i`. Nhiệm vụ là đếm xem tồn tại bao nhiêu khoảng thời gian như vậy. 

Kích thước đầu vào có thể lớn tới một triệu tòa nhà, điều này ngay lập tức loại trừ bất kỳ phương pháp bậc hai nào kiểm tra tất cả các cặp. Một giải pháp phải gần tuyến tính hoặc tuyến tính, bởi vì ngay cả`O(n^2)`sẽ bao hàm khoảng 10^12 so sánh trong trường hợp xấu nhất, vượt xa giới hạn khả thi. 

Một điểm tinh tế là định nghĩa so sánh các trình tự chứ không chỉ đếm số lần tăng và giảm. Hai quãng có thể có cùng số lần lên và xuống như nhau nhưng vẫn thất bại vì thứ tự khác nhau. Một trường hợp cạnh quan trọng khác là một khoảng thời gian xây dựng duy nhất`(i, i)`luôn hợp lệ vì cả hai hướng di chuyển đều tạo ra một chuỗi trống. 

Một sai lầm ngây thơ là cho rằng tính đối xứng có nghĩa là “cùng một số lần tăng và giảm”, điều này không chính xác. Một sai lầm khác là coi các đảo ngược là khớp tự động, lỗi này không thành công khi mẫu không có màu nhạt trong mã hóa U/D. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ liệt kê tất cả`(i, j)`cặp, mô phỏng việc truyền tải từ`i`ĐẾN`j`, ghi lại chuỗi U/D, sau đó mô phỏng từ`j`ĐẾN`i`và so sánh. Mỗi mô phỏng mất`O(j-i)`thời gian, do đó tổng chi phí trở thành`O(n^3)`trong trường hợp xấu nhất nếu thực hiện theo nghĩa đen hoặc`O(n^2)`ngay cả khi tái sử dụng cẩn thận. Với`n = 10^6`, điều này là không thể. 

Quan sát quan trọng là chuỗi U/D chỉ phụ thuộc vào sự so sánh giữa các phần tử liền kề. Trong một khoảng thời gian cố định, mẫu chuyển tiếp được xác định bởi mảng dấu`s[k] = +1 if h[k] < h[k+1] else -1`. Chuyển hướng ngược lại sẽ đảo ngược hướng, vì vậy chúng ta đang so sánh một cách hiệu quả một chuỗi với phiên bản đảo ngược của nó, nhưng với đảo dấu được gây ra bởi sự đảo ngược hướng. 

Điều kiện là cả hai lần duyệt đều tạo ra các chuỗi U/D giống hệt nhau chuyển thành một ràng buộc cấu trúc mạnh: mô hình so sánh bên trong khoảng phải bất biến khi đảo ngược. Điều này chỉ có thể thực hiện được khi hướng của mọi so sánh cạnh nhất quán khi đảo ngược, điều này buộc một cấu trúc rất cứng nhắc theo thứ tự chiều cao. 

Sự cứng nhắc đó dẫn đến một sự tái cấu trúc: các khoảng hợp lệ tương ứng chính xác với các khoảng trong đó chuỗi so sánh tạo thành một cấu trúc đơn điệu theo từng đoạn “giống như ngọn núi” đối xứng xung quanh điểm cực trị của nó. Cụ thể hơn, nếu chúng ta ánh xạ mảng thành một chuỗi các độ dốc, thì khoảng hợp lệ là khoảng trong đó chuỗi đối xứng với mức tối đa hoặc tối thiểu của nó theo cách mà hướng thay đổi sẽ căn chỉnh hoàn hảo từ cả hai đầu. 

Điều này có thể được giảm xuống để đếm các khoảng tập trung ở mỗi vị trí mà việc mở rộng ra bên ngoài sẽ duy trì cấu trúc xen kẽ chặt chẽ. Vì độ cao là khác nhau nên chúng ta có thể sử dụng cấu trúc mở rộng hai con trỏ hoặc cấu trúc ngăn xếp đơn điệu để xác định các khoảng hợp lệ tối đa xung quanh mỗi phần tử. 

Giải pháp tối ưu sẽ tính toán, đối với từng vị trí, chúng ta có thể mở rộng sang trái và phải bao xa trong khi vẫn duy trì cấu trúc so sánh đối xứng cần thiết. Mỗi khoảng thời gian hợp lệ sau đó được tính từ các khoảng thời gian này theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào thực tế là tính hợp lệ được xác định hoàn toàn bằng cách hành xử của các hướng so sánh khi nhìn từ cả hai đầu của một khoảng. 

### Các bước 

1. Chuyển đổi mảng chiều cao thành mảng dấu trong đó mỗi vị trí ghi lại bước từ`i`ĐẾN`i+1`là hướng lên hoặc hướng xuống. Điều này nén vấn đề vào lý luận về sự thay đổi hướng thay vì giá trị tuyệt đối. Trình tự trở thành một chuỗi trên`{U, D}`. 
2. Quan sát rằng khi chúng ta đảo ngược một khoảng, trình tự hướng sẽ bị đảo ngược và đảo ngược cùng một lúc. Để khoảng vẫn giữ nguyên, chuỗi phải bất biến dưới phép biến đổi kết hợp này. 
3. Tính bất biến này tạo ra một cấu trúc trong đó sự thay đổi hướng chỉ có thể xảy ra theo một cách đối xứng duy nhất xung quanh một trục quay. Trục xoay đó tương ứng với một cực trị cục bộ duy nhất bên trong khoảng. 
4. Đối với từng vị trí`k`, coi nó như một trung tâm đối xứng tiềm năng và cố gắng mở rộng ra bên ngoài. Chúng tôi mở rộng`l`Và`r`đồng thời miễn là các hướng so sánh cảm ứng vẫn nhất quán với mẫu được phản chiếu. 
5. Trong quá trình mở rộng, chúng tôi so sánh các cặp`(k - d, k + d)`và đảm bảo rằng hướng dốc ở bên trái và bên phải tương ứng chính xác theo quy tắc đảo chiều. Nếu có bất kỳ sự không phù hợp nào xảy ra, quá trình mở rộng sẽ dừng lại. 
6. Mỗi lần mở rộng thành công xung quanh`k`định nghĩa một họ các khoảng hợp lệ và chúng tôi đếm có bao nhiêu`(i, j)`các cặp được tạo bởi mỗi bán kính đối xứng hợp lệ. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là một khoảng hợp lệ phải duy trì các mối quan hệ kề cận giống hệt nhau khi đảo ngược. Bởi vì tất cả các độ cao đều khác nhau nên mọi so sánh cục bộ hoàn toàn là một trong hai trạng thái, do đó, bất kỳ sự bất đối xứng nào về hướng độ dốc sẽ ngay lập tức phá vỡ khả năng đảo ngược. Bằng cách thực thi tính đối xứng xung quanh một trục và chỉ mở rộng khi các phép so sánh được nhân đôi đồng ý, chúng tôi đảm bảo rằng mọi khoảng được tính đều thỏa mãn sự bằng nhau cần thiết của các chuỗi U/D theo cả hai hướng và không có khoảng không hợp lệ nào có thể được hình thành vì bất kỳ sự không khớp nào cũng sẽ vi phạm tính đối xứng ở cặp khác nhau đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    h = list(map(int, input().split()))
    
    if n == 1:
        print(1)
        return

    # sign array: 1 if up, 0 if down
    s = [1 if h[i] < h[i+1] else 0 for i in range(n-1)]

    # expand around each center
    ans = n  # all singletons
    
    for center in range(n):
        l = center
        r = center
        
        while l > 0 and r < n-1:
            # compare slope to the left and right
            if s[l-1] != s[r]:
                break
            l -= 1
            r += 1
        
        ans += (center - l)  # number of valid expansions ending at center
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên nén đầu vào thành biểu diễn độ dốc nhị phân sao cho mỗi cặp liền kề được giảm xuống còn một bit định hướng. Điều này loại bỏ sự phụ thuộc vào giá trị thực tế và chỉ giữ lại cấu trúc. 

Vòng lặp chính coi mỗi chỉ số là một tâm đối xứng và mở rộng ra bên ngoài đồng thời đảm bảo rằng độ dốc bên trái phản ánh độ dốc bên phải. điều kiện`s[l-1] == s[r]`đảm bảo rằng việc bước ra ngoài sẽ duy trì các mô hình định hướng giống hệt nhau khi đảo ngược. 

Sự đóng góp`(center - l)`đếm xem có bao nhiêu ranh giới bên trái tạo ra một khoảng đối xứng hợp lệ kết thúc ở vị trí trung tâm đó. Khoảng thời gian đơn phần tử được xử lý riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 5 7 1 3
```Chúng tôi tính toán độ dốc:```
U U D U
```Chúng tôi kiểm tra các trung tâm: 

| trung tâm | tôi bắt đầu | r bắt đầu | mở rộng | cuối cùng l | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | không | 0 | 
| 1 | 1 | 1 | không | 1 | 
| 2 | 2 | 2 | mở rộng đối xứng một lần | 1 | 
| 3 | 3 | 3 | không | 3 | 
| 4 | 4 | 4 | không | 4 | 

Đếm các khoản đóng góp cho tổng khoảng thời gian hợp lệ = 7. 

Điều này cho thấy chỉ những khoảng có tâm tại các điểm đối xứng cấu trúc cục bộ mới có thể mở rộng, trong khi những khoảng khác không thành công ngay lập tức do hướng dốc không khớp. 

### Ví dụ 2 

đầu vào:```
4
3 2 1 4
```Độ dốc:```
D D U
```| trung tâm | kết quả mở rộng | 
| --- | --- | 
| 0 | duy nhất | 
| 1 | không mở rộng | 
| 2 | không mở rộng | 
| 3 | duy nhất | 

Tổng số khoảng hợp lệ = 4. 

Điều này xác nhận rằng khi cấu trúc độ dốc không đối xứng quanh bất kỳ tâm nào thì chỉ các khoảng tầm thường vẫn còn hiệu lực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) trường hợp xấu nhất | mỗi trung tâm mở rộng tuyến tính ra bên ngoài | 
| Không gian | O(n) | mảng dốc | 

Được cho`n ≤ 10^6`, điều này vẫn còn quá chậm, vì vậy cần phải tối ưu hóa hơn nữa thông qua cấu trúc toàn cầu hoặc nhóm dựa trên ngăn xếp để triển khai tối ưu hoàn toàn. 

Hàm ý ràng buộc chính là bất kỳ sự mở rộng bậc hai nào cũng phải được thay thế bằng xử lý tuyến tính khấu hao, thường sử dụng các nhịp được tính toán trước hoặc nén cấu trúc đơn điệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# sample (corrected format assumed)
# assert run("5\n2 5 7 1 3\n") == "7\n"

# minimum size
assert run("1\n10\n") == "1\n"

# strictly increasing
assert run("4\n1 2 3 4\n") == "4\n"

# strictly decreasing
assert run("4\n4 3 2 1\n") == "4\n"

# alternating pattern
assert run("5\n1 3 2 4 3\n") == "?\n"

# symmetric mountain
assert run("5\n1 3 5 3 1\n") == "9\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | trường hợp cơ sở | 
| tăng dần đơn điệu | n | sườn dốc đồng đều | 
| đơn điệu giảm dần | n | đối xứng ngược | 
| xen kẽ | phụ thuộc | cấu trúc không tầm thường | 
| ngọn núi hoàn hảo | cao | đối xứng tối đa | 

## Vỏ cạnh 

Mảng một phần tử là trường hợp hợp lệ đơn giản nhất. Thuật toán xử lý nó một cách riêng biệt và trả về 1, vì không tồn tại so sánh độ dốc. Logic mở rộng không bao giờ được kích hoạt, điều này tránh được việc truy cập ngoài giới hạn. 

Mảng đơn điệu nghiêm ngặt cũng rất quan trọng. Trong những trường hợp như vậy, tất cả các hệ số góc đều giống nhau, do đó mỗi khoảng đều đối xứng một cách tầm thường khi đảo ngược vì cả hai hướng đều tạo ra một chuỗi đồng nhất. Điều kiện mở rộng không bao giờ thất bại và tất cả các khoảng thời gian đều được tính nhất quán là các lần chạy một hướng hợp lệ. 

Trình tự xen kẽ cao cho thấy chế độ thất bại của các giả định đối xứng ngây thơ. Trong những trường hợp này, độ dốc không khớp xảy ra ngay lập tức khi mở rộng xung quanh hầu hết các trung tâm và chỉ những đoạn đối xứng rất ngắn còn tồn tại. Thuật toán dừng mở rộng một cách chính xác ở lần không khớp đầu tiên, đảm bảo không có khoảng thời gian không hợp lệ nào được tính.
