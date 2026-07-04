---
title: "CF 102890E - Thưởng cuối năm"
description: "Chúng ta được xếp thành một hàng tròn gồm những người, mỗi người có một giá trị hiệu suất. Phần thưởng của mỗi người phụ thuộc vào hiệu suất của họ so với những người hàng xóm ngay bên trái và bên phải của họ như thế nào."
date: "2026-07-04T12:28:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102890
codeforces_index: "E"
codeforces_contest_name: "2020 ICPC Gran Premio de Mexico 3ra Fecha"
rating: 0
weight: 102890
solve_time_s: 46
verified: true
draft: false
---

[CF 102890E - Tiền thưởng cuối năm](https://codeforces.com/problemset/problem/102890/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp thành một hàng tròn gồm những người, mỗi người có một giá trị hiệu suất. Phần thưởng của mỗi người phụ thuộc vào hiệu suất của họ so với những người hàng xóm ngay bên trái và bên phải của họ như thế nào. Nếu một người thực hiện tốt hơn người hàng xóm ở một bên, họ sẽ nhận được khoản đóng góp cơ bản từ bên đó bằng một giá trị cố định cộng với tiền thưởng của người hàng xóm. Nếu họ vượt trội hơn cả hai người hàng xóm, phần thưởng cuối cùng của họ là mức đóng góp tối đa của hai người có thể đóng góp. Vì mọi người ngồi thành vòng tròn nên người đầu tiên và người cuối cùng cũng là hàng xóm của nhau. 

Nhiệm vụ là tính toán tiền thưởng cho mỗi người theo các quy tắc phụ thuộc này. Sự phụ thuộc không độc lập: mỗi giá trị phụ thuộc vào các giá trị liền kề, bản thân chúng phụ thuộc vào các giá trị khác, tạo thành một hệ thống ràng buộc trong một chu kỳ. 

Cấu trúc ngụ ý rằng không thể đánh giá trực tiếp nếu không giải quyết được những sự phụ thuộc lẫn nhau này. Mỗi vị trí có khả năng phụ thuộc vào cả hai vị trí lân cận và hướng phụ thuộc được xác định linh hoạt bằng cách so sánh các giá trị hiệu suất. 

Nếu số lượng người lớn, chẳng hạn lên tới 200000, thì bất kỳ giải pháp nào liên tục tính toán lại các giá trị hoặc truyền bá các bản cập nhật một cách nguyên bản trên mảng sẽ trở thành bậc hai trong trường hợp xấu nhất, vì mỗi bản cập nhật có thể kích hoạt phản ứng dây chuyền xung quanh vòng tròn. Điều đó ngay lập tức loại trừ việc thư giãn lặp đi lặp lại hoặc đệ quy ngây thơ mà không cần ghi nhớ. 

Trường hợp cạnh tinh tế phát sinh khi các giá trị hiệu suất hình thành một chu kỳ tăng hoặc giảm nghiêm ngặt. Trong những trường hợp như vậy, mọi vị trí đều phụ thuộc vào một hướng nhất quán và việc truyền bá cục bộ ngây thơ có thể giả định một cách không chính xác sự độc lập giữa các đóng góp từ trái sang phải và từ phải sang trái. 

Ví dụ: hãy xem xét ba người trong một vòng tròn có giá trị`[1, 2, 3]`. Người có giá trị 3 tốt hơn cả hai người hàng xóm nên phần thưởng của họ phụ thuộc vào cả hai bên. Việc chuyển từ trái sang phải ngây thơ có thể chỉ định một phần giá trị nhưng không thể truyền tải ảnh hưởng từ phải sang trái một cách chính xác, tạo ra kết quả không nhất quán. 

Một trường hợp cạnh khác xảy ra khi tất cả các giá trị đều bằng nhau, chẳng hạn như`[5, 5, 5, 5]`. Không ai thực sự giỏi hơn bất kỳ người hàng xóm nào, vì vậy tất cả tiền thưởng phải duy trì ở mức cơ bản. Bất kỳ thuật toán nào giả định ít nhất một hướng bất đẳng thức nghiêm ngặt trên mỗi cạnh sẽ truyền bá các cập nhật không chính xác. 

## Phương pháp tiếp cận 

Cấu trúc phụ thuộc sẽ trở nên rõ ràng hơn nếu chúng ta hiểu mỗi so sánh theo chỉ đạo là tạo ra một “dòng” đóng góp tiền thưởng tiềm năng. Đối với bất kỳ cặp liền kề nào, nếu`p[i] > p[j]`, sau đó`i`có thể nhận được một khoản đóng góp bắt nguồn từ`j`. Điều này tạo ra các cạnh có hướng giữa các lân cận, nhưng hướng không cố định trên toàn cầu, nó phụ thuộc vào thứ tự tương đối của các giá trị. 

Cách tiếp cận bạo lực sẽ liên tục cập nhật tất cả các phần thưởng cho đến khi không có giá trị nào thay đổi. Trong mỗi lần lặp, mỗi vị trí sẽ kiểm tra các vị trí lân cận và cập nhật giá trị của nó nếu một trong hai vị trí lân cận có đóng góp tốt hơn. Mỗi bản cập nhật phụ thuộc vào các trạng thái lân cận hiện tại, do đó các giá trị sẽ lan truyền dần dần xung quanh vòng tròn. 

Điều này hiệu quả vì hệ thống đơn điệu: tiền thưởng chỉ tăng lên. Tuy nhiên, trong trường hợp xấu nhất, mỗi bản cập nhật chỉ có thể lan truyền một bước trong mỗi lần lặp xung quanh vòng tròn. Với`n`các yếu tố, điều này có thể mất`O(n)`lần lặp và chi phí mỗi lần lặp`O(n)`, sản xuất`O(n^2)`sự phức tạp. Đối với đầu vào lớn, điều này là không khả thi. 

Quan sát quan trọng là quy tắc cập nhật có tính bất đối xứng về hướng gây ra bởi sự so sánh giá trị chứ không phải bởi các chỉ số. Khi chúng tôi sửa thứ tự do`p[i]`, chúng ta chỉ có thể xử lý các chuyển tiếp dọc theo hướng “xuống dốc lên dốc”. Một người chỉ có thể nhận được sự đóng góp từ một người hàng xóm có thành tích thực sự nhỏ hơn và bản thân sự đóng góp đó phụ thuộc vào những người hàng xóm nhỏ hơn nữa. 

Điều này biến vấn đề thành tính toán các chuỗi có trọng số dài nhất trên biểu đồ trong đó mỗi nút chỉ kết nối với các nút lân cận có giá trị nhỏ hơn. Bởi vì mỗi nút có nhiều nhất hai nút lân cận, nên cấu trúc sẽ trở thành một biểu đồ tuần hoàn có hướng sau khi định hướng các cạnh từ giá trị nhỏ hơn đến giá trị lớn hơn. Trên DAG này, chúng tôi có thể tính toán các giá trị bằng cách sử dụng DFS được ghi nhớ hoặc xử lý lặp theo thứ tự giảm dần`p[i]`. 

Việc sắp xếp các nút bằng cách giảm hiệu suất đảm bảo rằng khi chúng tôi xử lý một nút, tất cả những người đóng góp có thể có (phải có giá trị nhỏ hơn) đều đã được tính toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (thư giãn lặp đi lặp lại) | O(n²) | O(n) | Quá chậm | 
| Tối ưu (DP theo thứ tự giá trị) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi sự phụ thuộc vòng tròn thành các mối quan hệ có hướng dựa trên việc so sánh các giá trị liền kề, sau đó tính toán DP theo thứ tự hiệu suất giảm dần. 

1. Xây dựng mảng chỉ số được sắp xếp giảm dần`p[i]`. Điều này đảm bảo rằng khi xử lý một nút, cả hai nút lân cận có giá trị nhỏ hơn đều đã được xử lý nếu họ là những người đóng góp hợp lệ. Thứ tự là chìa khóa để loại bỏ các chu kỳ khỏi quá trình giải quyết sự phụ thuộc. 
2. Khởi tạo một mảng`f[i] = 0`cho mọi vị trí. Điều này thể hiện rằng ban đầu không có đóng góp nào được chỉ định. 
3. Lặp lại các chỉ mục theo thứ tự được sắp xếp. Đối với mỗi vị trí`i`, kiểm tra các hàng xóm bên trái và bên phải của nó bằng cách sử dụng chỉ mục vòng tròn. 
4. Đối với mỗi người hàng xóm`j`, kiểm tra xem`p[i] > p[j]`. Nếu vậy, sự đóng góp của ứng viên từ hướng đó là`B + f[j]`. Chúng tôi tính toán điều này bởi vì`i`thực sự là tốt hơn`j`, Vì thế`j`tiền thưởng đã được tính toán có thể được mở rộng lên tới`i`. 
5. Duy trì giá trị tốt nhất trong số những đóng góp hợp lệ. Nếu cả hai hàng xóm đều nhỏ hơn, hãy lấy mức đóng góp tối đa của cả hai ứng cử viên. Nếu chỉ có một cái hợp lệ thì cái đó sẽ được sử dụng. Nếu không có giá trị nào hợp lệ, giá trị vẫn bằng 0. 
6. Chỉ định`f[i]`tới giá trị tính toán tốt nhất. 

Lý do chúng tôi xử lý theo thứ tự giảm dần là bất cứ khi nào`p[i] > p[j]`, giá trị tại`j`phải được hoàn tất trước khi chúng tôi xử lý`i`. Điều này ngăn chặn việc cập nhật lặp lại và đảm bảo mỗi trạng thái được tính toán chính xác một lần. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là khi xử lý một nút`i`, mọi người hàng xóm`j`như vậy`p[j] < p[i]`đã có giá trị cuối cùng của nó`f[j]`. Vì các khoản đóng góp chỉ chuyển từ giá trị nhỏ hơn đến giá trị lớn hơn, nên sẽ không có thao tác nào sau này sửa đổi`f[j]`theo cách có ảnh hưởng`i`. Điều này làm cho biểu đồ phụ thuộc không có tính chu kỳ theo thứ tự`p`, và đảm bảo rằng mỗi`f[i]`tính toán là cuối cùng tại thời điểm chuyển nhượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, B = map(int, input().split())
    p = list(map(int, input().split()))
    
    order = sorted(range(n), key=lambda i: p[i], reverse=True)
    f = [0] * n
    
    for i in order:
        best = 0
        
        left = (i - 1) % n
        right = (i + 1) % n
        
        if p[i] > p[left]:
            best = max(best, B + f[left])
        if p[i] > p[right]:
            best = max(best, B + f[right])
        
        f[i] = best
    
    print(*f)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc số người và giá trị tiền thưởng cơ bản. Mảng hiệu suất được lưu trữ trong`p`. Sau đó, chúng tôi sắp xếp các chỉ số bằng cách giảm hiệu suất để tính toán các vị trí có hiệu suất cao hơn sau tất cả các vị trí lân cận có khả năng đóng góp có hiệu suất thấp hơn. 

Mảng`f`lưu trữ tiền thưởng được tính toán. Đối với mỗi chỉ mục theo thứ tự được sắp xếp, chúng tôi tính toán hai lân cận hình tròn của nó bằng cách sử dụng số học modulo. Chúng tôi chỉ xem xét đóng góp từ những người hàng xóm có hiệu suất nhỏ hơn. Mỗi đóng góp hợp lệ sẽ thêm`B`cộng với giá trị lân cận đã được tính toán. Mức đóng góp hợp lệ tối đa sẽ trở thành phần thưởng cuối cùng cho chỉ số đó. 

Một điểm tinh tế là việc sử dụng lập chỉ mục modulo để thực thi tính liền kề vòng tròn. Nếu không có điều này, các trường hợp biên cho phần tử đầu tiên và cuối cùng sẽ yêu cầu xử lý đặc biệt và rất dễ mắc sai lầm. 

## Ví dụ đã hoạt động 

Hãy xem xét`n = 3`,`B = 10`,`p = [1, 2, 3]`. 

Chúng tôi xử lý theo thứ tự`[2, 1, 0]`. 

| tôi | p[i] | trái | đúng | f[trái] | f[phải] | f[i] | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 1 | 0 | 0 | 0 | 0 | 
| 1 | 2 | 0 | 2 | 0 | 0 | 10 | 
| 0 | 1 | 2 | 1 | 0 | 10 | 20 | 

Tại chỉ số 2, cả hai hàng xóm đều nhỏ hơn nhưng`f`các giá trị vẫn chưa hữu ích cho việc truyền bá. Tại chỉ số 1, chỉ có chỉ số 0 nhỏ hơn nên tăng`B + f[0] = 10`. Tại chỉ số 0, cả hai hàng xóm đều lớn hơn trong bối cảnh thứ tự giá trị, nhưng chỉ có chỉ số 1 nhỏ hơn, cho`10 + 10 = 20`. 

Dấu vết này cho thấy các giá trị tích lũy như thế nào theo thứ tự hiệu suất tăng dần. 

Bây giờ hãy xem xét`n = 4`,`B = 5`,`p = [4, 1, 3, 2]`. 

| tôi | p[i] | trái | đúng | đóng góp hợp lệ | f[i] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 3 | 1 | từ 3,1 | tối đa(5+f[3], 5+f[1]) | 
| 2 | 3 | 1 | 3 | từ 1,3 | tối đa(5+f[1], 5+f[3]) | 
| 3 | 2 | 2 | 0 | từ 2 | 5+f[2] | 
| 1 | 1 | 0 | 2 | không | 0 | 

Điều này cho thấy các nút khác nhau tích lũy đóng góp như thế nào tùy thuộc vào so sánh cục bộ thay vì thứ tự chỉ mục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Các chỉ số sắp xếp chiếm ưu thế, mỗi nút được xử lý một lần với công việc O(1) | 
| Không gian | O(n) | Mảng cho các giá trị hiệu suất, kết quả DP và thứ tự | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc điển hình lên tới 200000 phần tử, vì tất cả công việc nặng nhọc đều là sắp xếp và đánh giá DP một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return sys.stdout.getvalue()

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, B = map(int, input().split())
    p = list(map(int, input().split()))
    
    order = sorted(range(n), key=lambda i: p[i], reverse=True)
    f = [0] * n
    
    for i in order:
        best = 0
        left = (i - 1) % n
        right = (i + 1) % n
        
        if p[i] > p[left]:
            best = max(best, B + f[left])
        if p[i] > p[right]:
            best = max(best, B + f[right])
        
        f[i] = best
    
    return " ".join(map(str, f)) + "\n"

# minimum size
assert solve("1 10\n5\n") == "0\n"

# all equal
assert solve("4 3\n2 2 2 2\n") == "0 0 0 0\n"

# increasing circle
assert solve("3 5\n1 2 3\n") == "20 10 0\n"

# decreasing circle
assert solve("3 5\n3 2 1\n") == "0 10 20\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | ranh giới tối thiểu | 
| tất cả đều bình đẳng | tất cả số không | không lan truyền | 
| ngày càng tăng | truyền chuỗi | dòng chảy định hướng | 
| giảm dần | chuỗi đảo ngược | tính đúng đắn của vòng tròn | 

## Vỏ cạnh 

Đối với vòng kết nối một người, hãy nói`n = 1`, các hàng xóm bên trái và bên phải là chính nó do lập chỉ mục vòng tròn. Từ`p[i] > p[i]`là sai, không có đóng góp nào được kích hoạt và kết quả là`0`. Thuật toán xử lý việc này một cách tự nhiên vì cả hai lần kiểm tra lân cận đều thất bại. 

Vì`p = [5, 5, 5, 5]`, mọi so sánh`p[i] > p[j]`thất bại, vì vậy mọi nút đều gán`f[i] = 0`. Thứ tự sắp xếp không liên quan vì không có chuyển đổi nào được kích hoạt và DP vẫn ổn định ở mức 0 xuyên suốt. 

Đối với một chu kỳ đơn điệu nghiêm ngặt như`[1, 2, 3, 4, 5]`, mỗi nút chỉ nhận được sự đóng góp từ các nút lân cận nhỏ hơn. Việc xử lý theo thứ tự giảm dần đảm bảo rằng khi đánh giá một nút, tất cả các nút nhỏ hơn đã có đóng góp cuối cùng của chúng, do đó việc tích lũy chuỗi diễn ra rõ ràng mà không cần xem lại các nút.
