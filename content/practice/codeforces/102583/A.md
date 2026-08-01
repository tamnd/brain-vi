---
title: "CF 102583A - \u0424\u043e\u0442\u043e\u0433\u0440\u0430\u0444\u0438\u0438 \u043d\u0430 \u043f\u0430\u043c\u044f\u0442\u044c"
description: "Chúng tôi có một nhóm sinh vật có chiều cao nhất định. Nhiếp ảnh gia muốn chia chúng thành số lượng ảnh nhỏ nhất có thể. Một bức ảnh có thể chứa một, hai hoặc ba sinh vật, nhưng chênh lệch chiều cao cho phép phụ thuộc vào số lượng sinh vật trong ảnh."
date: "2026-07-31T07:17:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102583
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102583
solve_time_s: 544
verified: true
draft: false
---

[CF 102583A - \u0424\u043e\u0442\u043e\u0433\u0440\u0430\u0444\u0438\u0438 \u043d\u0430 \u043f\u0430\u043c\u044f\u0442\u044c](https://codeforces.com/problemset/problem/102583/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 4 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm sinh vật có chiều cao nhất định. Nhiếp ảnh gia muốn chia chúng thành số lượng ảnh nhỏ nhất có thể. Một bức ảnh có thể chứa một, hai hoặc ba sinh vật, nhưng chênh lệch chiều cao cho phép phụ thuộc vào số lượng sinh vật trong ảnh. 

Đối với hai sinh vật, chênh lệch chiều cao của chúng tối đa là 20. Đối với ba sinh vật, chênh lệch giữa sinh vật thấp nhất và cao nhất trong số chúng phải lớn nhất là 10. Một sinh vật luôn có thể được chụp ảnh một mình. 

Đầu vào chứa số lượng sinh vật và chiều cao của chúng. Nhiệm vụ là xuất ra số lượng ảnh tối thiểu cần thiết để bao gồm tất cả mọi người đúng một lần. 

Số lượng sinh vật nhiều nhất là 1000 và mỗi chiều cao nằm trong khoảng từ 100 đến 1000. Điều này có nghĩa là giải pháp O(n^2) vẫn có thể được chấp nhận vì một triệu thao tác là nhỏ, nhưng chúng ta vẫn nên tìm kiếm cấu trúc đơn giản hơn vì vấn đề có thuộc tính sắp xếp. Việc tìm kiếm vũ lực trên tất cả các nhóm có thể sẽ là không thể vì số cách phân chia 1000 sinh vật tăng lên cực kỳ nhanh chóng. 

Các trường hợp đặc biệt quan trọng đến từ sự khác biệt giữa giới hạn của hai và ba sinh vật. Một nhóm ba người có thể không hợp lệ ngay cả khi mọi cặp bên trong đều có vẻ chấp nhận được. Ví dụ:```
3
100 110 120
```Câu trả lời đúng là 2. Ba sinh vật không thể chia sẻ một bức ảnh vì tổng chiều cao là 20, nhưng hai cặp 100 và 110, 110 và 120 đều có thể được chụp cùng nhau. 

Một trường hợp khác là khi chỉ có hai sinh vật đủ gần:```
3
100 115 200
```Câu trả lời đúng là 2. Một giải pháp bất cẩn chỉ kiểm tra xem chênh lệch chiều cao nhỏ nhất và lớn nhất có đủ nhỏ hay không có thể loại bỏ sai cặp 100 và 115 và tạo ra 3 ảnh. 

Trường hợp ranh giới cuối cùng là khi tất cả các sinh vật có cùng chiều cao:```
4
500 500 500 500
```Câu trả lời đúng là 2 vì mỗi nhóm ba sinh vật đều hợp lệ, nhưng một bức ảnh không thể chứa bốn sinh vật. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi cách có thể để chia các sinh vật thành các nhóm một, hai và ba, kiểm tra xem mỗi nhóm có đáp ứng các quy tắc về ảnh hay không. Cách tiếp cận này đúng vì nó xem xét mọi cách sắp xếp có thể và chọn số lượng ảnh nhỏ nhất. Tuy nhiên, số lượng phân vùng là rất lớn. Ngay cả khi chỉ có 1000 sinh vật, việc khám phá tất cả các nhóm vẫn vượt xa giới hạn khả thi. 

Quan sát hữu ích xuất phát từ thực tế là chỉ có sự khác biệt về chiều cao mới quan trọng. Sau khi sắp xếp độ cao, những sinh vật đủ gần nhau sẽ xuất hiện cạnh nhau. Không có lợi ích gì khi đặt một sinh vật thấp hơn cùng với một sinh vật cao hơn nhiều trong khi để yên một sinh vật gần đó. 

Sau khi sắp xếp, bài toán trở thành bài toán lập trình động trên các tiền tố. Gọi dp[i] là số lượng ảnh tối thiểu cần thiết cho sinh vật được tôi sắp xếp đầu tiên. Khi coi sinh vật i là sinh vật cuối cùng trong tiền tố, chỉ có ba kết thúc có thể xảy ra: bức ảnh cuối cùng chứa một sinh vật, hai sinh vật hoặc ba sinh vật. Đây là những trường hợp duy nhất vì một bức ảnh không thể chứa nhiều hơn ba sinh vật. 

Quá trình chuyển đổi kiểm tra xem hai hoặc ba chiều cao được sắp xếp cuối cùng có đáp ứng các giới hạn yêu cầu hay không. Lấy mức tối thiểu thay vì các lựa chọn hợp lệ sẽ mang lại câu trả lời tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Lập trình động sau khi sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các chiều cao theo thứ tự không giảm. Sau khi sắp xếp, bất kỳ nhóm sinh vật hợp lệ nào cũng có thể được coi là một phân đoạn liên tiếp, bởi vì việc tách các độ cao gần nhau trong khi nhóm các sinh vật ở xa hơn không thể cải thiện số lượng ảnh. 
2. Tạo một mảng lập trình động trong đó dp[i] đại diện cho số lượng ảnh tối thiểu cần thiết cho i sinh vật đầu tiên theo thứ tự được sắp xếp. 
3. Khởi tạo dp[0] bằng 0 vì không sinh vật nào không cần ảnh. 
4. Với mỗi vị trí i từ 1 đến n, trước tiên chỉ lấy sinh vật thứ i mà thôi. Điều này mang lại sự chuyển đổi dp[i] = dp[i - 1] + 1. 
5. Nếu hai sinh vật cuối cùng tạo thành một cặp hợp lệ, hãy cập nhật dp[i] bằng cách sử dụng dp[i - 2] + 1. Cặp này hợp lệ khi chênh lệch chiều cao giữa chúng tối đa là 20. 
6. Nếu ba sinh vật cuối cùng tạo thành một nhóm hợp lệ, hãy cập nhật dp[i] bằng cách sử dụng dp[i - 3] + 1. Bộ ba hợp lệ khi chênh lệch giữa chiều cao lớn nhất và nhỏ nhất trong ba chiều cao này tối đa là 10. 
7. Giá trị dp[n] là câu trả lời vì nó mô tả số lượng ảnh tối thiểu cần thiết cho tất cả các sinh vật được sắp xếp. 

Tại sao nó hoạt động: 

Trạng thái lập trình động xem xét mọi cách có thể để hình thành bức ảnh cuối cùng. Bất kỳ giải pháp tối ưu nào cho sinh vật thứ nhất đều phải kết thúc bằng một, hai hoặc ba sinh vật trong bức ảnh cuối cùng. Việc xóa ảnh cuối cùng đó sẽ để lại giải pháp tối ưu cho tiền tố nhỏ hơn, vì nếu không, chúng tôi có thể thay thế ảnh đó bằng ảnh tốt hơn và cải thiện giải pháp ban đầu. Quá trình chuyển đổi kiểm tra chính xác những kết thúc có thể có này, vì vậy dp[i] luôn lưu trữ mức tối thiểu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    inf = 10**9
    dp = [inf] * (n + 1)
    dp[0] = 0

    for i in range(1, n + 1):
        dp[i] = dp[i - 1] + 1

        if i >= 2 and a[i - 1] - a[i - 2] <= 20:
            dp[i] = min(dp[i], dp[i - 2] + 1)

        if i >= 3 and a[i - 1] - a[i - 3] <= 10:
            dp[i] = min(dp[i], dp[i - 3] + 1)

    print(dp[n])

if __name__ == "__main__":
    solve()
```Bước sắp xếp đặt các độ cao tương tự cạnh nhau, cho phép quá trình chuyển đổi lập trình động chỉ kiểm tra một số sinh vật cuối cùng. Nếu không sắp xếp, một nhóm hợp lệ có thể trải rộng trên mảng và các chuyển đổi cục bộ sẽ không thể hiện được tất cả các khả năng. 

Mảng có kích thước n + 1 vì dp[0] đại diện cho tiền tố trống. Việc kiểm tra các cặp và bộ ba sử dụng i làm số lượng sinh vật được xử lý, vì vậy chỉ mục được xử lý cuối cùng là i - 1. Các điều kiện sử dụng các chỉ mục này một cách cẩn thận để tránh truy cập các phần tử trước khi bắt đầu mảng. 

Quá trình chuyển đổi sinh vật đơn lẻ luôn có sẵn, vì vậy mọi trạng thái đều nhận được giá trị hợp lệ ban đầu. Chuyển đổi cặp và chuyển đổi ba chỉ cải thiện giá trị đó khi giới hạn chiều cao của chúng được thỏa mãn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
100 300 200
```Sau khi sắp xếp, chiều cao là 100, 200, 300. 

| tôi | Chiều cao đã xử lý | Tùy chọn ảnh đơn | Tùy chọn ghép nối | Tùy chọn ba | dp[i] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | không | 0 | chưa được kiểm tra | chưa được kiểm tra | 0 | 
| 1 | 100 | 1 | chưa được kiểm tra | chưa được kiểm tra | 1 | 
| 2 | 100, 200 | 2 | không hợp lệ, chênh lệch 100 | chưa được kiểm tra | 2 | 
| 3 | 100, 200, 300 | 3 | không hợp lệ | không hợp lệ, phạm vi 200 | 3 | 

Ba sinh vật ở cách nhau quá xa nên mỗi con cần có một bức ảnh riêng. 

Đối với mẫu thứ hai:```
3
110 120 130
```Thứ tự sắp xếp đã là 110, 120, 130. 

| tôi | Chiều cao đã xử lý | Tùy chọn ảnh đơn | Tùy chọn ghép nối | Tùy chọn ba | dp[i] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | không | 0 | chưa được kiểm tra | chưa được kiểm tra | 0 | 
| 1 | 110 | 1 | chưa được kiểm tra | chưa được kiểm tra | 1 | 
| 2 | 110, 120 | 2 | hợp lệ, cho 1 | chưa được kiểm tra | 1 | 
| 3 | 110, 120, 130 | 2 | hợp lệ, cho 2 | không hợp lệ, phạm vi 20 | 2 | 

Hai sinh vật đầu tiên có thể chia sẻ một bức ảnh và sinh vật thứ ba tham gia bức ảnh còn lại với một trong hai người hàng xóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong quy trình lập trình động tuyến tính | 
| Không gian | O(n) | Mảng dp lưu trữ một giá trị cho mỗi tiền tố | 

Với n bằng 1000, nghiệm này dễ dàng nằm trong giới hạn. Bước sắp xếp là thao tác tốn kém nhất, trong khi phần lập trình động chỉ thực hiện một lượng công việc không đổi cho mỗi sinh vật. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    dp = [10**9] * (n + 1)
    dp[0] = 0

    for i in range(1, n + 1):
        dp[i] = dp[i - 1] + 1
        if i >= 2 and a[i - 1] - a[i - 2] <= 20:
            dp[i] = min(dp[i], dp[i - 2] + 1)
        if i >= 3 and a[i - 1] - a[i - 3] <= 10:
            dp[i] = min(dp[i], dp[i - 3] + 1)

    sys.stdin = old_stdin
    return str(dp[n])

assert solve_data("3\n100 300 200\n") == "3"
assert solve_data("3\n110 120 130\n") == "2"
assert solve_data("6\n100 210 250 255 220 260\n") == "3"

assert solve_data("1\n500\n") == "1"
assert solve_data("4\n500 500 500 500\n") == "2"
assert solve_data("3\n100 115 200\n") == "2"
assert solve_data("6\n100 101 102 103 104 105\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 500`| 1 | Đầu vào nhỏ nhất có thể | 
|`4 / 500 500 500 500`| 2 | Được phép nhóm ba người, nhóm bốn người thì không | 
|`3 / 100 115 200`| 2 | Cặp ranh giới có chênh lệch 15 | 
|`6 / 100 101 102 103 104 105`| 2 | Nhiều bộ ba hợp lệ và những cái bẫy trông có vẻ tham lam | 

## Vỏ cạnh 

Đối với trường hợp:```
3
100 110 120
```Sắp xếp không làm gì cả. Quá trình chuyển đổi ba lần kiểm tra phạm vi 120 - 100, tức là 20, do đó nhóm ba bị từ chối. Quá trình chuyển đổi cặp chấp nhận 100 và 110, cho hai ảnh. Thuật toán trả về 2. 

Đối với trường hợp:```
3
100 115 200
```Mảng được sắp xếp là 100, 115, 200. Cặp của hai sinh vật đầu tiên hợp lệ vì chênh lệch là 15. Sinh vật cuối cùng không thể tham gia một trong hai nhóm vì chênh lệch quá lớn. Các giá trị lập trình động trở thành 1 cho hai sinh vật đầu tiên và 2 sau khi thêm sinh vật thứ ba, cho kết quả chính xác. 

Đối với trường hợp:```
4
500 500 500 500
```Mọi cặp có thể và kiểm tra ba lần đều thành công. Sự sắp xếp tốt nhất là một sinh vật ba và một sinh vật đơn, vì các quy tắc giới hạn mỗi bức ảnh ở ba sinh vật. Các chuyển đổi quy hoạch động tìm dp[3] = 1 và sau đó dp[4] = 2, là tối ưu. 

Tôi cũng có thể cung cấp phiên bản biên tập theo phong cách Codeforces ngắn hơn nếu bạn muốn một phiên bản gần giống với những gì sẽ xuất hiện trên trang cuộc thi.
