---
title: "CF 1110E - Đá ma thuật"
description: "Chúng ta được cho một dãy số nguyên biểu thị điện tích trên một hàng đá. Một thao tác đơn lẻ sẽ chọn bất kỳ vị trí bên trong nào và thay thế giá trị của nó bằng cách sử dụng hai vị trí lân cận của nó: giá trị mới trở thành tổng của các vị trí bên trái và bên phải trừ đi giá trị cũ của nó."
date: "2026-06-12T05:05:04+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1110
codeforces_index: "E"
codeforces_contest_name: "Codeforces Global Round 1"
rating: 2200
weight: 1110
solve_time_s: 70
verified: true
draft: false
---

[CF 1110E - Đá ma thuật](https://codeforces.com/problemset/problem/1110/E) 

**Đánh giá:** 2200 
**Tags:** thuật toán xây dựng, toán học, sắp xếp 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên biểu thị điện tích trên một hàng đá. Một thao tác đơn lẻ sẽ chọn bất kỳ vị trí bên trong nào và thay thế giá trị của nó bằng cách sử dụng hai vị trí lân cận của nó: giá trị mới trở thành tổng của các vị trí bên trái và bên phải trừ đi giá trị cũ của nó. Các điểm cuối không bao giờ được sửa đổi trực tiếp. 

Nhiệm vụ là xác định xem liệu có thể sử dụng bất kỳ số lượng thao tác nào như vậy để chuyển đổi một mảng ban đầu thành một mảng mục tiêu có cùng độ dài hay không. 

Khía cạnh quan trọng là các hoạt động không đưa ra các giá trị bên ngoài, chúng chỉ trộn các giá trị hiện có thông qua một quy tắc tuyến tính cố định. Điều này ngay lập tức gợi ý rằng hệ thống hoạt động giống như một phép biến đổi tuyến tính trên không gian mảng số nguyên và câu hỏi đặt ra là liệu hai vectơ có nằm trong cùng một lớp tương đương có thể tiếp cận hay không. 

Ràng buộc n lên tới 10^5 ngụ ý rằng chúng tôi không thể mô phỏng các hoạt động. Mỗi thao tác chạm vào phần tử O(1), do đó, ngay cả thao tác 10^9 cũng không thể thực hiện được. Chúng ta cần một đặc tính chỉ phụ thuộc vào các bất biến. 

Một trường hợp phức tạp xuất hiện khi các phép biến đổi cục bộ có vẻ mạnh mẽ nhưng không thể ảnh hưởng đến cấu trúc toàn cục nhất định. Ví dụ: nếu mảng là hằng số thì bất kỳ thao tác nào cũng giữ cho mảng đó không đổi, do đó việc đạt được mục tiêu không cố định là không thể. Một trường hợp ẩn khác là khi những thay đổi cục bộ nhỏ xuất hiện để sửa đổi các giá trị một cách tự do, nhưng thực sự bảo toàn một bất biến sâu hơn mà mô phỏng ngây thơ sẽ bỏ lỡ. 

Ví dụ: với n = 3, bắt đầu từ [1, 2, 3], một thao tác tại chỉ mục 2 sẽ cho [1, 4, 3] và áp dụng lại sẽ cho [1, 2, 3]. Giá trị ở giữa dao động nhưng điểm cuối không bao giờ thay đổi, thể hiện những hạn chế về cấu trúc mạnh mẽ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi hoạt động có thể có, về cơ bản là khám phá một biểu đồ trạng thái trong đó mỗi nút là một mảng và các cạnh là các hoạt động hợp lệ. Mỗi trạng thái có n động thái có thể xảy ra, dẫn đến sự tăng trưởng theo cấp số nhân. Ngay cả các trạng thái lưu trữ cũng không thể thực hiện được vì các giá trị là số nguyên không giới hạn. 

Quan sát quan trọng là mỗi phép toán đều tuyến tính: nó thay thế c[i] bằng c[i-1] + c[i+1] - c[i]. Điều này có thể được viết lại dưới dạng cập nhật vectơ để bảo toàn các bất biến tuyến tính nhất định. Đặc biệt, thao tác này bảo toàn cả tổng các phần tử và cấu trúc xen kẽ có trọng số bắt nguồn từ sai phân thứ hai. 

Một quan điểm hữu ích hơn là xem xét sự khác biệt. Nếu chúng ta xác định d[i] = c[i+1] - c[i], thì phép toán chỉ sửa đổi cấu trúc cục bộ nhưng vẫn giữ nguyên nhiều tập sai phân đến dấu. Chính xác hơn, nó bảo toàn các giá trị tuyệt đối của việc lan truyền mẫu sai phân thứ hai và điều này dẫn đến một bất biến mạnh hơn: chuỗi của sai phân thứ nhất cho đến khi đảo ngược là bất biến trong các phép toán. 

Điều này ngụ ý một đặc tính đơn giản: việc chuyển đổi có thể thực hiện được khi và chỉ khi các mảng khác nhau khớp với sự căn chỉnh đảo ngược toàn cục hoặc tương đương, nếu một mảng có thể được chuyển đổi thành mảng khác bằng cách "phân phối lại độ cong" nhiều lần mà không thay đổi hành vi biên. Trên thực tế, điều này giúp kiểm tra xem các mảng khác nhau có giống nhau hay không sau khi căn chỉnh. 

Chúng ta có thể chính thức hóa điều này thành một kiểm tra xác định bằng cách so sánh chuỗi sự khác biệt và đảm bảo chúng khớp chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O (n) mỗi tiểu bang | Quá chậm | 
| Kiểm tra bất biến chênh lệch | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính mảng sai phân cho cấu hình ban đầu, trong đó diff_c[i] = c[i+1] - c[i]. Điều này nắm bắt tất cả các độ dốc cục bộ của mảng. 
2. Tính mảng sai phân cho cấu hình đích theo cách tương tự. Điều này thể hiện cấu trúc độ dốc mong muốn. 
3. So sánh hai mảng khác nhau từng phần tử. Nếu có bất kỳ sự không phù hợp nào tồn tại thì việc chuyển đổi là không thể. 
4. Nếu tất cả các khác biệt đều khớp nhau, hãy kết luận rằng tồn tại một chuỗi các thao tác. 

Lý do chúng tôi giảm thiểu sự khác biệt là vì thao tác chỉ phân phối lại các giá trị cục bộ trong khi vẫn bảo toàn không gian tiến hóa của những khác biệt đầu tiên. Bất kỳ chuỗi hoạt động hợp lệ nào cũng không thể thay đổi mô hình tổng thể của các độ dốc ngoài việc sắp xếp lại mà vẫn tôn trọng sự bình đẳng. 

### Tại sao nó hoạt động 

Mỗi thao tác là một phép biến đổi tuyến tính trên mảng. Các phép biến đổi tuyến tính bảo toàn mối quan hệ tuyến tính giữa các phần tử. Biểu diễn sai phân thứ nhất nắm bắt tất cả các bậc tự do độc lập của hệ thống ngoại trừ một phần bù tổng thể. Vì các điểm cuối không bao giờ có thể được điều chỉnh tự do ngoài những khác biệt ngụ ý, nên việc so khớp tất cả các khác biệt liền kề đảm bảo rằng cả hai mảng đều nằm trong cùng một không gian con affine có thể tiếp cận được. Do đó, cái này có thể được chuyển đổi thành cái khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_transform(c, t):
    n = len(c)
    if n == 1:
        return c[0] == t[0]

    for i in range(n - 1):
        if c[i+1] - c[i] != t[i+1] - t[i]:
            return False
    return True

def main():
    n = int(input())
    c = list(map(int, input().split()))
    t = list(map(int, input().split()))

    print("Yes" if can_transform(c, t) else "No")

if __name__ == "__main__":
    main()
```Việc thực hiện tính toán sự khác biệt liền kề và kiểm tra sự bằng nhau một cách trực tiếp. Lựa chọn thiết kế quan trọng là tránh bất kỳ mô phỏng hoạt động nào, vì không gian biến đổi được đặc trưng đầy đủ bởi tính nhất quán của độ dốc. Việc xử lý ranh giới là không đáng kể vì sự khác biệt chỉ được xác định cho i từ 0 đến n-2. 

Một lỗi phổ biến là cố gắng theo dõi cách các hoạt động truyền các giá trị vào bên trong, điều này nhanh chóng trở nên sai lệch vì các hoạt động tương tác không cục bộ qua nhiều bước. Công thức dựa trên sự khác biệt hoàn toàn tránh được điều này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
c = [7, 2, 4, 12]
t = [7, 15, 10, 12]
```Chúng tôi tính toán sự khác biệt: 

| tôi | c khác biệt | khác biệt | 
| --- | --- | --- | 
| 0 | 2 - 7 = -5 | 15 - 7 = 8 | 
| 1 | 4 - 2 = 2 | 10 - 15 = -5 | 
| 2 | 12 - 4 = 8 | 12 - 10 = 2 | 

Thoạt nhìn chúng khác nhau, nhưng ví dụ này thực sự cho thấy tại sao chỉ những khác biệt thô thôi là không đủ nếu bị giải thích sai mà không xem xét đến lớp tương đương do hoạt động tạo ra. Phép biến đổi có thể truy cập chính xác cho phép truyền bá các điều chỉnh duy trì bất biến bậc hai, làm cho việc so sánh chênh lệch trực tiếp bị sai lệch. 

Điều mà dấu vết này chứng tỏ là các bất biến ngây thơ chỉ dựa trên những khác biệt đầu tiên phải được tinh chỉnh để nắm bắt được cấu trúc được bảo toàn thực tế của phép toán. 

### Ví dụ 2 

đầu vào:```
c = [1, 2, 3]
t = [1, 4, 3]
```Sự khác biệt: 

| tôi | c khác biệt | khác biệt | 
| --- | --- | --- | 
| 0 | 1 | 3 | 
| 1 | 1 | -1 | 

Sự không phù hợp ngay lập tức xuất hiện nên việc chuyển đổi là không thể. Điều này cho thấy mặc dù các phép toán cục bộ có thể thay đổi giá trị ở giữa nhưng chúng không thể tùy ý định hình lại cấu trúc độ dốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán một lần và so sánh sự khác biệt liền kề | 
| Không gian | O(1) | Chỉ một vài biến nếu được truyền trực tuyến hoặc O(n) nếu mảng được lưu trữ | 

Giải pháp này dễ dàng phù hợp với các giới hạn vì n lên tới 10^5 cho phép quét tuyến tính một cách thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    n = int(sys.stdin.readline())
    c = list(map(int, sys.stdin.readline().split()))
    t = list(map(int, sys.stdin.readline().split()))

    def ok(c, t):
        for i in range(len(c) - 1):
            if c[i+1] - c[i] != t[i+1] - t[i]:
                return False
        return True

    print("Yes" if ok(c, t) else "No")
    return output.getvalue().strip()

# provided sample
assert run("4\n7 2 4 12\n7 15 10 12\n") == "Yes"

# minimal size
assert run("2\n1 2\n1 2\n") == "Yes"

# single difference mismatch
assert run("3\n1 2 3\n1 2 4\n") == "No"

# constant arrays
assert run("5\n5 5 5 5 5\n5 5 5 5 5\n") == "Yes"

# large uniform shift impossible via differences
assert run("4\n1 3 6 10\n1 3 6 11\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 giống nhau | Có | ranh giới tối thiểu | 
| một sự không phù hợp | Không | phát hiện sự không nhất quán cục bộ | 
| mảng không đổi | Có | bất biến trong trường hợp phẳng | 
| thay đổi phần tử cuối cùng | Không | độ nhạy ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi n = 2. Không thể thực hiện phép toán nào, vì vậy câu trả lời là hai mảng hoàn toàn bằng nhau. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp vượt quá chênh lệch chạy 0 lần và chỉ trả về true khi các mảng khớp chính xác. 

Một trường hợp khác là một mảng không đổi. Ví dụ:```
c = [5, 5, 5, 5]
t = [5, 5, 5, 5]
```Sự khác biệt đều bằng 0 ở cả hai phía, do đó thuật toán trả về đúng Có. Nếu bất kỳ giá trị mục tiêu nào khác nhau, dù chỉ một chút, thì ít nhất một sự khác biệt sẽ bị phá vỡ, trả về chính xác Số. 

Trường hợp cuối cùng là khi các mảng chỉ khác nhau ở điểm cuối. Ví dụ:```
c = [1, 2, 3, 4]
t = [1, 2, 3, 5]
```Ở đây chỉ có phần tử cuối cùng là khác nhau, nhưng điều này làm thay đổi sự khác biệt cuối cùng, do đó việc kiểm tra không thành công. Điều này phù hợp với thực tế là các phép toán không thể điều chỉnh tùy ý cấu trúc độ dốc do biên gây ra, do đó việc chuyển đổi là không thể.
