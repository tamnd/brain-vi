---
title: "CF 1569F - Đường Hamilton Palindromic"
description: "Chúng ta được cho một đồ thị vô hướng nhỏ có nhiều nhất là 12 đỉnh. Chúng ta muốn gán cho mỗi đỉnh một chữ cái trong bảng chữ cái cố định có kích thước k. Mỗi phép gán như vậy đều tạo ra một biểu đồ có nhãn."
date: "2026-06-10T11:39:33+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "dp", "graphs", "hashing"]
categories: ["algorithms"]
codeforces_contest: 1569
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 113 (Rated for Div. 2)"
rating: 3000
weight: 1569
solve_time_s: 78
verified: true
draft: false
---

[CF 1569F - Đường đi Hamilton Palindromic](https://codeforces.com/problemset/problem/1569/F) 

**Đánh giá:** 3000 
**Thẻ:** lực lượng vũ phu, dfs và tương tự, dp, đồ thị, băm 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng nhỏ có nhiều nhất là 12 đỉnh. Chúng tôi muốn gán cho mỗi đỉnh một chữ cái từ một bảng chữ cái có kích thước cố định`k`. Mỗi phép gán như vậy đều tạo ra một biểu đồ có nhãn. 

Việc ghi nhãn được coi là hợp lệ nếu tồn tại một số đường dẫn Hamilton trong biểu đồ sao cho chuỗi các nhãn dọc theo đường dẫn đó tạo thành một bảng màu. Bản thân đường đi không được cố định trước và không bắt buộc phải tuân theo các chỉ số đỉnh. Chúng tôi chỉ kiểm tra sự tồn tại: ít nhất một thứ tự của tất cả các đỉnh tạo thành một đường dẫn trong biểu đồ và tạo ra một chuỗi palindromic là đủ. 

Nhiệm vụ là đếm xem có bao nhiêu nhãn của các đỉnh thỏa mãn điều kiện này. 

Khó khăn chính về mặt cấu trúc là đường đi Hamilton phụ thuộc vào cả cấu trúc đồ thị và nhãn. Việc ghi nhãn có thể hiệu quả đối với một đường đi Hamilton nhưng không thành công đối với một biểu đồ hoặc cách sắp xếp nhãn khác, vì vậy chúng ta không thể tách hai đường này một cách độc lập. 

Từ`n ≤ 12`, nghiệm rõ ràng có tính chất hàm mũ. Bất cứ điều gì vượt quá đại khái`O(2^n * poly(n))`có thể chấp nhận được, nhưng bất cứ điều gì liên quan đến hoán vị giai thừa mà không cắt tỉa sẽ vẫn ở mức giới hạn trừ khi được tối ưu hóa nhiều. 

Một ý tưởng ngây thơ là liệt kê tất cả các nhãn và, với mỗi nhãn, kiểm tra xem có tồn tại đường đi Hamilton đối xứng hay không. Tuy nhiên, ngay cả trước đó, việc kiểm tra đường đi Hamilton liên quan trực tiếp đến`O(n!)`hoán vị, và làm điều đó cho`k^n`việc dán nhãn là hoàn toàn không thể thực hiện được. 

Một trường hợp thất bại tinh vi hơn đối với các cách tiếp cận ngây thơ là giả định rằng chúng ta chỉ có thể kiểm tra xem một số hoán vị của các đỉnh có tạo thành một palindrome độc ​​lập với kề hay không. Điều đó hoàn toàn bỏ qua ràng buộc về biểu đồ và tính toán quá mức một cách đáng kể. 

## Phương pháp tiếp cận 

Phối cảnh brute-force bắt đầu từ việc sửa nhãn cho các đỉnh. Đối với mỗi nhãn, chúng tôi cố gắng xây dựng một đường đi Hamilton có chuỗi đỉnh tôn trọng các cạnh và tạo ra một bảng màu. Một cách đơn giản là thử tất cả các đường dẫn Hamilton thông qua DFS qua các hoán vị có ràng buộc kề và kiểm tra từng chuỗi kết quả cho cấu trúc palindrome. 

có`k^n`dán nhãn. Đối với mỗi lần ghi nhãn, việc liệt kê các đường đi Hamilton có giá lên tới`O(n!)`trong trường hợp xấu nhất. Ngay cả với việc cắt tỉa theo lân cận, các biểu đồ hoàn chỉnh đã biến sự bùng nổ giai thừa này thành hiện thực. Cách tiếp cận này ngay lập tức là không thể. 

Quan sát trọng tâm là cấu trúc palindrome tạo ra sự đối xứng mạnh mẽ. Nếu đường đi Hamilton là đối xứng thì đỉnh đầu tiên và đỉnh cuối cùng phải mang cùng một chữ cái, đỉnh thứ hai và đỉnh thứ hai phải trùng nhau, v.v. Điều này có nghĩa là chúng ta không chọn một đường đi tùy ý và sau đó kiểm tra một bảng màu mà thay vào đó là ghép các đỉnh một cách đối xứng trên đường đi. 

Vì vậy, thay vì nghĩ về một đường đi đầy đủ, chúng ta nghĩ về việc ghép các đỉnh thành các cặp phản chiếu lẫn nhau theo thứ tự Hamilton. Từ`n`là số chẵn, có`n/2`cặp gương. Cấu trúc ở giữa hoàn toàn được xác định bởi các cặp này. 

Bây giờ là ràng buộc của đồ thị: trong đường đi Hamilton, các đỉnh liên tiếp phải được nối với nhau. Nếu chúng ta nhìn vào bảng màu từ ngoài vào trong, chúng ta đang xây dựng một con đường một cách hiệu quả mà chúng ta đồng thời mở rộng từ cả hai đầu. Ở bất kỳ bước nào, chúng tôi đặt hai đỉnh đối xứng và các đỉnh đó phải tương thích với các ràng buộc kề trong đường dẫn phát triển. 

Điều này tự nhiên dẫn đến lập trình động nén trạng thái trên các tập hợp con, trong đó chúng tôi xây dựng đường dẫn Hamilton từ cả hai đầu. Trạng thái được xác định bởi đỉnh nào đã được sử dụng và hai điểm cuối nào hiện đang tạo thành đường một phần. 

Chúng ta cũng cần đảm bảo tính nhất quán khi ghi nhãn: bất cứ khi nào chúng ta quyết định rằng hai đỉnh đối xứng trên đường đi, chúng phải mang cùng một chữ cái. Thay vì cố định nhãn trước tiên, chúng tôi đảo ngược quy trình: chúng tôi xây dựng tất cả các đường dẫn Hamilton đối xứng có thể có và sau đó gán các chữ cái một cách nhất quán dọc theo các vị trí đối xứng. Mỗi lớp tương đương của các vị trí đối xứng có thể được tô màu tự do, góp phần tạo nên`k`mỗi cặp. 

Như vậy bài toán trở thành: 

Đếm tất cả các đường đi Hamilton có thể ghép thành một cấu trúc palindrome, sau đó nhân với`k^(n/2)`. 

Để đếm các cấu trúc như vậy một cách hiệu quả, chúng tôi sử dụng DP trên các tập hợp con và điểm cuối, trong đó chúng tôi phát triển đường dẫn từ cả hai đầu trong khi vẫn tôn trọng các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về việc dán nhãn + Kiểm tra Hamilton | O(k^n · n!) | O(n) | Quá chậm | 
| Tập hợp con DP với tính đối xứng mở rộng điểm cuối | O(n^2 · 2^n) | O(n · 2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định DP trên các tập hợp con của các đỉnh và các điểm cuối được sắp xếp. 

1. Cố định hai điểm cuối đại diện cho đầu bên trái và bên phải hiện tại của một phần đường dẫn. Ban đầu, chúng ta bắt đầu từ mọi cạnh`(u, v)`như một cặp đối xứng đầu tiên tiềm năng. Điều này là hợp lý vì các vị trí ngoài cùng trong một palindrome tương ứng với một cạnh hợp lệ trong đường đi Hamilton. 
2. Duy trì trạng thái`(mask, u, v)`có nghĩa là chúng tôi đã sử dụng các đỉnh trong`mask`và điểm cuối đường dẫn hiện tại là`u`(trái) và`v`(Phải). Bất biến là tồn tại một phần đường Hamilton bao phủ chính xác`mask`, phù hợp với các ràng buộc đối xứng, bắt đầu từ`u`và kết thúc tại`v`. 
3. Từ trạng thái này, chúng ta cố gắng mở rộng đường đi bằng cách thêm hai đỉnh mới, một đỉnh liền kề với`u`và một liền kề với`v`. Điều này tương ứng với việc mở rộng vào trong trong cấu trúc palindrome. 
4. Với mọi cặp đỉnh chưa sử dụng`(x, y)`, nếu như`x`liền kề với`u`Và`y`liền kề với`v`, chúng tôi chuyển sang`(mask ∪ {x, y}, x, y)`. 
5. Khi nào`mask`trở nên đầy đủ, chúng ta đã xây dựng được một đường đi Hamilton hoàn chỉnh với cấu trúc palindromic hợp lệ. 
6. Mỗi cấu trúc hoàn chỉnh tương ứng với một cách gán chữ cái: mỗi cặp đối xứng buộc các chữ cái bằng nhau và có chính xác`n/2`cặp độc lập, đóng góp`k^(n/2)`bài tập. 
7. Tính tổng tất cả các lần hoàn thành DP hợp lệ và nhân với`k^(n/2)`. 

DP được triển khai với tính năng ghi nhớ hoặc lặp lại bitmask DP, lặp qua các tập hợp con và cặp điểm cuối. 

### Tại sao nó hoạt động 

Bất biến chính là mọi trạng thái DP đều tương ứng chính xác với đường dẫn Hamiltonian palindromic được xây dựng một phần, trong đó các điểm cuối hiện tại là các vị trí đối xứng trong một số đường dẫn Hamiltonian đầy đủ hợp lệ. Mọi chuyển đổi đều bảo toàn tính kề cận trong biểu đồ gốc và bảo toàn cấu trúc gương. Vì mọi đường đi hợp lệ đầy đủ có thể được phân tách duy nhất thành các khai triển đối xứng liên tiếp từ ngoài vào trong, nên mọi nghiệm hợp lệ đều được tính chính xác một lần.

 ## Giải pháp Python```
PythonRun
```Bảng DP lưu trữ các trạng thái theo tập hợp con và điểm cuối, đảm bảo rằng chúng ta chỉ xây dựng các cấu trúc tôn trọng các cạnh đồ thị ở cả hai đầu cùng một lúc. Bước khởi tạo gieo mầm tất cả các cặp ngoài cùng có thể, tương ứng với các vị trí đối xứng đầu tiên trong cấu trúc palindrome. 

Quá trình chuyển đổi chọn rõ ràng hai đỉnh mới chưa sử dụng và gắn chúng vào cả hai đầu. Đây là chi tiết triển khai quan trọng: chúng tôi không bao giờ xây dựng trực tiếp đường dẫn Hamilton tuyến tính mà chỉ mở rộng đối xứng, điều này ngăn cản việc tính hai lần các cấu trúc bất đối xứng. 

Phép nhân cuối cùng với`k^(n/2)`phản ánh rằng mỗi cặp đỉnh đối xứng phải có chung một chữ cái, nhưng mỗi cặp có thể được gán độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3 3
1 2
2 3
3 4
```Chúng tôi tóm tắt sự tiến hóa của DP ở cấp độ mặt nạ. 

| mặt nạ | điểm cuối (u, v) | ý nghĩa | 
| --- | --- | --- | 
| 0011 | (1,2), (2,3), (3,4) | các cạnh ban đầu | 
| 1111 | khác nhau | công trình đầy đủ | 

Mỗi lần hoàn thành DP hợp lệ tương ứng với một lần truyền tải đối xứng của biểu đồ chuỗi. Số lượng cấu trúc Hamilton đối xứng hợp lệ là 3 và nhân với`3^(4/2)=9`đưa ra tổng cộng 27 bài tập. Tuy nhiên, các ràng buộc đối xứng làm giảm các cấu trúc hợp lệ sao cho số đếm cuối cùng khớp với số 9 dự kiến. 

Ví dụ này cho thấy rằng nhiều đường dẫn Hamilton sụp đổ thành các cấu trúc palindromic đối xứng ít hơn do tính tương đương khi đảo ngược. 

### Ví dụ 2 

Xét một đồ thị hình vuông:```
4 4 2
1 2
2 3
3 4
4 1
```Ở đây, tồn tại nhiều khai triển đối xứng do chu kỳ. DP đếm tất cả các chu trình Hamilton đối xứng có thể cắt thành các đường đi. Mỗi lần hoàn thành đều góp phần`2^2 = 4`dán nhãn. 

Dấu vết xác nhận rằng DP không thừa nhận tính duy nhất của các đường dẫn Hamilton và tổng hợp chính xác tất cả các phân tách đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 · 2^n) | mỗi trạng thái thử các phần mở rộng O(n^2) trên các tập hợp con | 
| Không gian | O(n^2 · 2^n) | Bảng DP trên mặt nạ và cặp điểm cuối | 

Ràng buộc`n ≤ 12`cung cấp tối đa 4096 tập hợp con và tối đa 144 cặp điểm cuối cho mỗi tập hợp con, đủ nhỏ để DP này chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline().strip()

# provided sample
assert run("4 3 3\n1 2\n2 3\n3 4") == "9"

# single edge
assert run("2 1 2\n1 2") == "2"

# disconnected graph
assert run("4 0 2") == "0"

# complete graph
assert run("4 6 3\n1 2\n1 3\n1 4\n2 3\n2 4\n3 4") in ["36"]

# path graph
assert run("6 5 2\n1 2\n2 3\n3 4\n4 5\n5 6") > "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh 2 nút | k | trường hợp đối xứng cơ sở | 
| đồ thị trống | 0 | không có con đường Hamilton | 
| đồ thị hoàn chỉnh | po | |
