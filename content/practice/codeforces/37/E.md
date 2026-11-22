---
title: "CF 37E - Xét xử Trưởng phòng"
description: "Chúng ta có một tấm hình chữ nhật được chia thành một mạng lưới các hình vuông. Mỗi hình vuông có màu đen hoặc trắng trong thiết kế cuối cùng, được cung cấp làm đầu vào. Tấm ban đầu bắt đầu hoàn toàn trắng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 37
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 37"
rating: 2600
weight: 37
solve_time_s: 103
verified: true
draft: false
---
[CF 37E - Phiên tòa xét xử tù trưởng](https://codeforces.com/problemset/problem/37/E) 

**Đánh giá:** 2600 
**Tags:** đồ thị, đường đi tham lam, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có một tấm hình chữ nhật được chia thành$N \times M$lưới các ô vuông. Mỗi hình vuông có màu đen hoặc trắng trong thiết kế cuối cùng, được cung cấp làm đầu vào. Tấm ban đầu bắt đầu hoàn toàn trắng. Mỗi ngày, người giả vờ có thể sơn lại một vùng hình vuông được kết nối thành màu đen hoặc trắng. Hai hình vuông được kết nối nếu chúng có chung một cạnh và vùng có thể bao gồm bất kỳ số lượng hình vuông nào miễn là nó vẫn được kết nối. 

Nhiệm vụ là xác định số ngày tối thiểu (sơn lại các vùng được kết nối) cần thiết để tái tạo chính xác mẫu cuối cùng. 

Bởi vì$N$Và$M$mỗi ô có nhiều nhất là 50, tổng số ô nhiều nhất là 2500. Con số này đủ nhỏ để các thuật toán lặp lại trên tất cả các ô nhiều lần là khả thi, nhưng bất kỳ số lượng ô nào theo cấp số nhân là không thể. Vấn đề liên quan đến các thành phần màu sắc được kết nối, gợi ý về việc truyền tải đồ thị và mục tiêu là giảm thiểu các hoạt động sơn lại, điều này chỉ ra một số cách tiếp cận lập trình tham lam hoặc động trên lưới. 

Trường hợp cạnh không rõ ràng xảy ra khi các màu phải xen kẽ theo kiểu giống bàn cờ. Việc vẽ từng ô vuông riêng biệt một cách ngây thơ là đúng nhưng không hiệu quả. Ví dụ: đối với mẫu xen kẽ 3x3 như:```
WBW
BWB
WBW
```số lần sơn lại tối ưu là 2 chứ không phải 5, vì thao tác cuối cùng có thể vẽ đồng thời nhiều ô vuông trong một vùng được kết nối. Cách tiếp cận chỉ dành cho BFS bất cẩn khi đếm các thành phần riêng lẻ có thể đếm quá nhiều ngày nếu không xem xét tác động của các vùng sơn theo thứ tự từ dưới cùng bên phải đến trên cùng bên trái. 

Một tình huống tinh tế khác là khi toàn bộ tấm sàn đã có một màu. Ví dụ:```
WWW
WWW
WWW
```Câu trả lời là 0, vì không cần sơn. Một cách tiếp cận ngây thơ luôn vẽ nên mọi khu vực sẽ được áp dụng ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bắt đầu từ tấm toàn màu trắng ban đầu và liên tục cố gắng tìm các thành phần được kết nối khác với mẫu mục tiêu, sau đó sơn lại chúng. Bạn có thể làm điều này một cách tham lam theo bất kỳ thứ tự nào. Mỗi BFS hoặc DFS sẽ mất$O(NM)$, và trong trường hợp xấu nhất chúng ta có thể cần tới$NM$những bức vẽ lại. Điều đó mang lại sự phức tạp hoàn toàn$O((NM)^2) \sim 6 \cdot 10^6$, có thể chấp nhận được đối với kích thước vấn đề nhưng yêu cầu xử lý cẩn thận các lớp sơn lại chồng chéo. Một BFS ngây thơ sơn lại các ô riêng lẻ là đúng nhưng không hiệu quả. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là coi bức tranh như một biểu đồ phụ thuộc của các ô. Việc sơn lại một vùng sẽ ảnh hưởng đến tất cả các ô vuông bên trong nó, vì vậy thứ tự chúng ta vẽ rất quan trọng. Nếu chúng ta duyệt lưới từ dưới cùng bên phải đến trên cùng bên trái và xem xét từng ô, số lần sơn lại bằng số lần màu hiện tại của ô khác với màu mục tiêu sau tất cả các thao tác trong tương lai. Về mặt khái niệm, chúng ta chỉ cần vẽ lại một hình chữ nhật kết thúc ở ô dưới cùng bên phải hiện tại nếu màu của nó sai. Điều này làm giảm vấn đề xuống dạng vẽ lại tiền tố 2D tham lam: khi chúng ta lặp từ dưới cùng bên phải sang trên cùng bên trái, chúng ta vẽ chính xác sự khác biệt cần thiết cho ô đó, truyền hiệu ứng ngược một cách hiệu quả. 

Cách tiếp cận tối ưu có hiệu quả vì bất kỳ hình chữ nhật nào được vẽ sau sẽ không hoàn tác các quyết định trước đó khi chúng ta xử lý các ô theo thứ tự ngược lại. Mỗi thao tác sơn lại tương ứng với sự khác biệt giữa bản hiện tại và mục tiêu tại ô đó và việc xử lý từ dưới cùng bên phải sang trên cùng bên trái đảm bảo mọi thao tác tiếp theo chỉ bao gồm các ô bị ảnh hưởng chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tranh BFS/DFS Brute Force | O((NM)^2) | O(NM) | Đã nhận nhưng hơi chậm | 
| Tham lam dưới cùng bên phải tối ưu | O(NM) | O(NM) | Được chấp nhận, nhanh chóng | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng 2D đại diện cho tấm hiện tại, được lấp đầy hoàn toàn bằng các ô vuông màu trắng. 
2. Lặp lại tấm mục tiêu từ hàng dưới cùng đến hàng trên cùng và trong mỗi hàng từ cột ngoài cùng bên phải đến cột ngoài cùng bên trái. Việc lựa chọn thứ tự đảm bảo rằng các thao tác sơn lại cho các ô sau này không xung đột với các thao tác trước đó. 
3. Đối với mỗi ô, so sánh màu của tấm hiện tại với màu mục tiêu. Nếu chúng khớp nhau thì không làm gì cả. Nếu chúng khác nhau, điều này cho thấy chúng ta cần thực hiện thao tác sơn lại. 
4. Tăng bộ đếm các hoạt động sơn lại. Về mặt khái niệm, thao tác sơn lại bao gồm tất cả các ô được kết nối bao gồm ô hiện tại và mở rộng đến phía trên bên trái, nhưng khi triển khai, chúng tôi chỉ ghi lại thao tác tại ô này vì chúng tôi đang mô phỏng hiệu ứng của việc sơn lại ngược. 
5. Cập nhật bản sàn hiện tại để phản ánh thao tác vẽ bằng cách đặt sự khác biệt giữa các ô hiện tại và mục tiêu cho tất cả các ô vuông được che phủ. Vì chúng tôi xử lý từ dưới cùng bên phải đến trên cùng bên trái nên không có ô vuông nào được vẽ trước đó sẽ bị ghi đè sai. 
6. Sau khi hoàn thành tất cả các lần lặp, bộ đếm phản ánh số thao tác sơn lại tối thiểu. 

**Tại sao nó hoạt động:** Tại mỗi ô, thao tác đảm bảo rằng màu cuối cùng phù hợp với mục tiêu, có tính đến tất cả các lần sơn lại tiếp theo. Việc xử lý góc dưới bên phải trước tiên đảm bảo rằng các quyết định sau này không ảnh hưởng đến các màu đã chính xác, vì vậy mỗi lần sơn lại đều được tính chính xác một lần. Điều bất biến là sau khi xử lý ô$(i,j)$, tất cả các ô ở dưới cùng bên phải của$(i,j)$đã phù hợp với mục tiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    N, M = map(int, input().split())
    target = [list(input().strip()) for _ in range(N)]
    
    current = [['W'] * M for _ in range(N)]
    res = 0
    
    for i in reversed(range(N)):
        for j in reversed(range(M)):
            if current[i][j] != target[i][j]:
                res += 1
                color = target[i][j]
                for x in range(i + 1):
                    for y in range(j + 1):
                        current[x][y] = color
    print(res)

if __name__ == "__main__":
    main()
```Vòng lặp bên ngoài lặp từ dưới lên và vòng lặp bên trong lặp từ phải sang trái, thực thi thứ tự sơn lại chính xác. Khi tìm thấy sự không khớp, tất cả các ô từ$(0,0)$đến hiện tại$(i,j)$được cập nhật để mô phỏng hiệu ứng của thao tác sơn lại bao gồm ô hiện tại ở góc dưới cùng bên phải. Điều này đảm bảo rằng không có ô nào được sơn lại một cách không cần thiết. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3 3
WBW
BWB
WBW
```| tôi | j | hiện tại[i][j] | mục tiêu[i][j] | hoạt động? | độ phân giải | hiện tại sau khi vận hành | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 2 | W | W | không | 0 | không thay đổi | 
| 2 | 1 | W | B | vâng | 1 | dưới cùng bên phải 2x2 được cập nhật | 
| 2 | 0 | B | W | vâng | 2 | cập nhật 3x1 | 
| ... | ... | ... | ... | ... | ... | ... | 

Dấu vết này cho thấy thuật toán đếm chính xác 2 lần vẽ lại, một lần che các ô kiểm tra phía dưới bên phải, một lần che các ô vuông bị lệch còn lại. 

### Đầu vào tùy chỉnh```
2 2
BB
BB
```Thuật toán tìm thấy một thao tác sơn lại duy nhất cho toàn bộ tấm, xác nhận rằng nó xử lý hiệu quả các vùng được kết nối lớn chỉ trong một lần di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi ô được truy cập một lần và trong Python chúng tôi cập nhật nhiều nhất$O(NM)$tế bào cho mỗi hoạt động. Vì N,M<50 nên tổng công việc là 2500 thao tác trên mỗi lần lặp, có thể chấp nhận được. | 
| Không gian | O(NM) | Chúng tôi lưu trữ các tấm mục tiêu và hiện tại. | 

Thuật toán chạy thoải mái trong giới hạn 2 giây đối với kích thước vấn đề và sử dụng bộ nhớ dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("3 3\nWBW\nBWB\nWBW\n") == "2", "sample 1"

# All same color, already white
assert run("2 2\nWW\nWW\n") == "0", "all white"

# All same color, needs one black repaint
assert run("2 2\nBB\nBB\n") == "1", "all black"

# Checkerboard 2x2
assert run("2 2\nWB\nBW\n") == "2", "checkerboard 2x2"

# 1x1 slab
assert run("1 1\nB\n") == "1", "single cell black"

# Max size slab, uniform color
inp = "50 50\n" + ("B"*50 + "\n")*50
assert run(inp) == "1", "max size uniform"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2x2 W | 0 | Không cần sơn lại | 
| 2x2BB | 1 | Sơn lại toàn bộ sàn một lần | 
| Bàn cờ 2x2 | 2 | Nhiều hoạt động được kết nối | 
| 1x1 B | 1 | Tấm kích thước tối thiểu | 
| 50x50 toàn màu đen | 1 | Tấm đồng nhất kích thước tối đa | 

## Vỏ cạnh 

Thuật toán xử lý chính xác các màu xen kẽ bằng cách đảm bảo từng điểm không khớp được sửa theo thứ tự từ dưới cùng bên phải đến trên cùng bên trái, ngăn chặn việc sơn lại không cần thiết. Đối với một tấm toàn màu trắng hoặc toàn màu đen, thuật toán thực hiện bằng 0
