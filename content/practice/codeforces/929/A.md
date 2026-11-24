---
title: "CF 929A - \u041f\u0440\u043e\u043a\u0430\u0442 \u0432\u0435\u043b\u043e\u0441\u0438\u043f\u0435\u0434\u043e\u0432"
description: "Chúng ta được cung cấp một chuỗi các vị trí dọc theo một đường thẳng nơi đặt các trạm xe đạp. Trạm đầu tiên trùng với điểm xuất phát của Arkady (trường học) và trạm cuối cùng trùng với điểm đến của Arkady (nhà)."
date: "2026-06-17T03:05:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 929
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2018 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f 2"
rating: 1400
weight: 929
solve_time_s: 70
verified: true
draft: false
---

[CF 929A - \u041f\u0440\u043e\u043a\u0430\u0442 \u0432\u0435\u043b\u043e\u0441\u0438\u043f\u0435\u0434\u043e\u0432](https://codeforces.com/problemset/problem/929/A) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, tham lam, thực hiện 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các vị trí dọc theo một đường thẳng nơi đặt các trạm xe đạp. Trạm đầu tiên trùng với điểm xuất phát của Arkady (trường học) và trạm cuối cùng trùng với điểm đến của Arkady (nhà). Giữa chúng là các trạm trung gian mà anh ta có thể sử dụng làm điểm dừng bắt buộc. 

Arkady chỉ di chuyển bằng xe đạp, nhưng mỗi lần thuê một chiếc xe đạp, anh ấy chỉ có thể đạp nó đi một quãng đường cố định`k`. Khi đến ga, anh ta phải trả lại chiếc xe đạp hiện tại, tùy ý lấy một chiếc mới và đi tiếp. Anh ta chỉ được phép đổi xe đạp ở những trạm này. Mục tiêu là để xác định xem liệu anh ta có thể đến được trạm cuối cùng hay không và nếu có thì số lượng xe đạp tối thiểu anh ta phải thuê, tính cả chiếc đầu tiên. 

Quan sát quan trọng là chuyển động bị hạn chế khi phải nhảy giữa các ga và mỗi chuyến đi phải đi qua một đoạn có chiều dài không vượt quá`k`. 

Những hạn chế`n ≤ 1000`và phối hợp lên đến`100000`có nghĩa là giải pháp O(n2) đã được chấp nhận. Tuy nhiên, cấu trúc thậm chí còn đơn giản hơn nên việc quét tham lam tuyến tính là đủ. Chúng tôi đang lựa chọn một cách hiệu quả số lượng phân đoạn tối thiểu bao phủ đường dẫn từ trái sang phải. 

Một trường hợp thất bại đơn giản xuất hiện khi ai đó cố gắng luôn đi đến trạm xa nhất có thể tiếp cận mà không cập nhật cẩn thận từ vị trí hiện tại chính xác. Ví dụ, nếu các trạm`0, 3, 6`Và`k = 4`, tham lam phải đi 0 → 3 → 6. Một cách tiếp cận có lỗi tính toán lại không chính xác khả năng tiếp cận từ nguồn sai có thể bỏ qua các chuyển đổi hợp lệ. 

Một trường hợp tinh tế khác là khi khoảng cách giữa các trạm liên tiếp vượt quá`k`. Ví dụ,`0, 5, 6`với`k = 4`khiến việc đi lại không thể thực hiện được mặc dù đoạn sau đủ nhỏ. Bất kỳ giải pháp chính xác nào cũng phải phát hiện điều này ngay lập tức. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là coi mỗi trạm như một nút và thử tất cả các chuỗi di chuyển hợp lệ có thể có. Từ ga`i`, chúng ta có thể di chuyển đến bất kỳ trạm nào`j > i`như vậy`x[j] - x[i] ≤ k`. Sau đó, chúng tôi cố gắng đệ quy để giảm thiểu số lần thuê cần thiết để đến được trạm cuối cùng. 

Điều này có hiệu quả vì bài toán là đường đi ngắn nhất trong đồ thị chu kỳ có hướng, nhưng việc khám phá tất cả các đường dẫn sẽ dẫn đến sự phân nhánh theo cấp số nhân trong trường hợp xấu nhất. Với`n = 1000`, điều này trở nên hoàn toàn không thể thực hiện được. 

Thông tin chi tiết về cấu trúc quan trọng là biểu đồ được sắp xếp theo thứ tự và các cạnh chỉ đi về phía trước. Điều này có nghĩa là một khi chúng ta quyết định đi xe đạp từ ga`i`, trạm tiếp theo tốt nhất có thể là trạm có thể tiếp cận xa nhất, bởi vì bất kỳ lựa chọn nào trước đó chỉ làm giảm phạm vi tiếp cận trong tương lai. Điều này biến vấn đề thành một cuộc đi bộ tham lam: liên tục nhảy xa nhất có thể trong khoảng cách`k`. 

Chúng ta chỉ cần duy trì một con trỏ đến trạm xa nhất có thể tiếp cận được từ vị trí hiện tại và di chuyển đến đó, đếm xem chúng ta sử dụng bao nhiêu đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các trạm theo thứ tự từ trái sang phải. 

1. Bắt đầu tại trạm 0 (chỉ số 0) với con trỏ vị trí hiện tại. 
2. Từ trạm hiện tại, quét về phía trước để tìm trạm xa nhất có khoảng cách tối đa với trạm hiện tại`k`. 
3. Nếu không có trạm như vậy thì chuyến đi không thể thực hiện được và chúng ta dừng ngay lập tức. 
4. Di chuyển đến trạm xa nhất có thể tiếp cận và tăng số lượng xe đạp lên 1. 
5. Lặp lại cho đến khi chúng ta đến trạm cuối cùng. 

Lý do chúng tôi luôn chọn trạm có thể tiếp cận xa nhất là vì bất kỳ lựa chọn nào gần hơn sẽ giảm nghiêm ngặt hoặc giữ nguyên nhóm có thể tiếp cận còn lại từ vị trí tiếp theo, không bao giờ mở rộng nó. Vì tất cả các quyết định trong tương lai chỉ phụ thuộc vào vị trí hiện tại nên việc tối đa hóa tiến độ trước mắt là tối ưu. 

### Tại sao nó hoạt động 

Tại bất kỳ trạm nào`i`, xác định tập hợp các trạm có thể truy cập là tất cả`j > i`như vậy`x[j] - x[i] ≤ k`. Mọi giải pháp hợp lệ đều phải chọn trạm tiếp theo từ tập hợp này. Nếu chúng ta chọn nơi xa nhất`j`, thì mọi trạm có thể truy cập được từ bất kỳ lựa chọn nhỏ hơn nào cũng có thể truy cập được từ`j`, bởi vì`x[j]`là tối đa. Điều này duy trì khả năng tiếp cận và giảm thiểu số lần chuyển tiếp, tương ứng trực tiếp với việc giảm thiểu việc thuê xe đạp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    x = list(map(int, input().split()))
    
    i = 0
    ans = 0
    
    while i < n - 1:
        j = i
        while j + 1 < n and x[j + 1] - x[i] <= k:
            j += 1
        
        if j == i:
            print(-1)
            return
        
        ans += 1
        i = j
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện giữ một con trỏ`i`cho trạm hiện tại. Vòng lặp bên trong tiến lên`j`càng xa càng tốt trong khi vẫn ở trong khoảng cách`k`từ`x[i]`. Nếu như`j`không di chuyển, điều đó có nghĩa là ngay cả trạm tiếp theo cũng không thể truy cập được, vì vậy câu trả lời là`-1`. 

Mỗi lần di chuyển thành công sẽ tăng số lần thuê xe đạp và nhảy thẳng đến trạm xa nhất có thể tiếp cận, đảm bảo chuyển đổi tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
3 6 8 10
```Chúng tôi bắt đầu ở chỉ số 0 (vị trí 3). 

| Hiện tại tôi | Vị trí hiện tại | Khoảng cách có thể tiếp cận xa nhất j | Hành động | Cho thuê | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 1 (6) | nhảy tới 1 | 1 | 
| 1 | 6 | 3 (10) | nhảy tới 3 | 2 | 

Chúng tôi đến ga cuối cùng trong 2 lần thuê, phù hợp với đầu ra. 

Điều này chứng tỏ trạm trung gian 2 (8) bị bỏ qua vì việc nhảy trực tiếp từ 6 lên 10 vẫn nằm trong phạm vi`k`. 

### Ví dụ 2 

đầu vào:```
2 9
0 10
```| Hiện tại tôi | Vị trí hiện tại | Khoảng cách có thể tiếp cận xa nhất j | Hành động | Cho thuê | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | không | không thể | - | 

Từ 0, trạm tiếp theo là 10, nhưng khoảng cách vượt quá`k = 9`, nên không thể di chuyển được. Thuật toán xuất ra chính xác`-1`. 

Điều này cho thấy lỗi xảy ra ngay lập tức khi ngay cả phân đoạn đầu tiên không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trạm được truy cập tối đa một lần dưới dạng con trỏ hiện tại và một lần khi quét | 
| Không gian | O(1) | Chỉ có một vài gợi ý được sử dụng | 

Quét tuyến tính dễ dàng phù hợp trong giới hạn vì`n ≤ 1000`và những ràng buộc thậm chí lớn hơn sẽ vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, k = map(int, input().split())
    x = list(map(int, input().split()))
    
    i = 0
    ans = 0
    
    while i < n - 1:
        j = i
        while j + 1 < n and x[j + 1] - x[i] <= k:
            j += 1
        if j == i:
            return "-1"
        ans += 1
        i = j
    
    return str(ans)

# provided sample
assert run("4 4\n3 6 8 10\n") == "2"

# minimum size impossible
assert run("2 1\n0 5\n") == "-1"

# minimum size possible
assert run("2 10\n0 5\n") == "1"

# all stations reachable in one go
assert run("5 100\n0 10 20 30 40\n") == "1"

# chain requiring many jumps
assert run("5 5\n0 3 6 9 12\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 / 0 5 | -1 | không thể ngay lập tức | 
| 2 10 / 0 5 | 1 | nhảy một lần là đủ | 
| chuỗi khoảng cách rộng | 1 | khả năng tiếp cận đầy đủ trong một chuyến đi | 
| khoảng cách đồng đều | 4 | tiến triển tuyến tính trong trường hợp xấu nhất | 

## Vỏ cạnh 

Khi hai trạm liên tiếp cách xa nhau hơn`k`, vòng lặp bên trong thất bại ngay lập tức vì`j`không thể di chuyển xa hơn`i`. Đối với đầu vào`2 3`với các vị trí`0 10`, thuật toán bắt đầu tại`i = 0`, thấy không có giá trị`j`, và trả về`-1`trực tiếp. Điều này phù hợp với yêu cầu Arkady không bao giờ đi bộ. 

Khi tất cả các trạm nằm trong một đoạn chiều dài`k`, chẳng hạn như`0 2 4 6`với`k = 10`, vòng lặp bên trong luôn đến trạm cuối cùng trong một bước. Thuật toán tăng dần`ans`một lần và kết thúc, giảm thiểu tiền thuê một cách chính xác. 

Khi những lựa chọn tham lam có vẻ mơ hồ, chẳng hạn như`0 3 5 7 10`với`k = 5`, thuật toán luôn đẩy tới trạm xa nhất có thể tiếp cận, tạo ra`0 → 5 → 10`, điều này tránh được việc thuê trung gian không cần thiết và xác nhận rằng bước nhảy tối đa cục bộ sẽ duy trì tính tối ưu toàn cầu.
