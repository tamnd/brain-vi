---
title: "CF 104101J - Trò chơi đơn giản"
description: "Chúng ta được cấp một dãy số nguyên và hai người chơi luân phiên lấy từng số một cho đến khi dãy số đó trống. Alice di chuyển đầu tiên. Mỗi người chơi tích lũy tổng các số họ đã chọn."
date: "2026-07-02T02:09:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104101
codeforces_index: "J"
codeforces_contest_name: "The 2022 Zhejiang University City College Freshman Programming Contest"
rating: 0
weight: 104101
solve_time_s: 44
verified: true
draft: false
---

[CF 104101J - Trò chơi đơn giản](https://codeforces.com/problemset/problem/104101/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên và hai người chơi luân phiên lấy từng số một cho đến khi dãy số đó trống. Alice di chuyển đầu tiên. Mỗi người chơi tích lũy tổng các số họ đã chọn. 

Sau khi lấy tất cả các số, chúng tôi tính toán chênh lệch tuyệt đối giữa tổng của Alice và tổng của Bob. Alice thắng nếu chênh lệch này là số lẻ, ngược lại Bob thắng. 

Khó khăn chính không phải là quá trình chọn, bởi vì người chơi có thể tự do lấy bất kỳ phần tử nào còn lại. Câu hỏi thực sự là liệu lối chơi tối ưu có ảnh hưởng đến sự ngang bằng cuối cùng của sự khác biệt hay không. 

Ràng buộc n có thể lên tới 1e6, do đó, bất kỳ cách tiếp cận nào mô phỏng các lựa chọn hoặc xem xét các tập hợp con hoặc trạng thái trò chơi đều không thể thực hiện được. Chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính, bởi vì ngay cả việc sắp xếp O(n log n) cũng được nhưng bất cứ thứ gì bậc hai hoặc hàm mũ thì không. 

Một điểm tinh tế là các giá trị có thể lớn, lên tới 1e9, nhưng cuối cùng chỉ có vấn đề tương đương. Điều đó thường báo hiệu rằng lời giải rút gọn thành lập luận modulo 2 thay vì tính tổng chính xác. 

Một cách tiếp cận ngây thơ sẽ cố gắng mô phỏng trò chơi hoặc mô hình hóa các chiến lược chọn hàng tối ưu. Ví dụ: người ta có thể nghĩ rằng người chơi nên luôn chọn phần tử lớn nhất còn lại. Điều đó đã dẫn đến suy luận sai vì cách chơi tối ưu không quan trọng đối với tính chẵn lẻ của chênh lệch cuối cùng trong bài toán này. Kết quả chỉ phụ thuộc vào sự bất biến về cấu trúc chứ không phụ thuộc vào việc ra quyết định. 

Một trực giác sai lầm khác là coi nó như một trò chơi tiêu chuẩn “lấy tối đa xen kẽ” và tính các tổng xen kẽ. Điều đó không thành công vì người chơi không bị ràng buộc ở các đầu của mảng hoặc bất kỳ cấu trúc đặt hàng nào. Bất kỳ yếu tố nào cũng có thể được lấy bất cứ lúc nào, điều này loại bỏ tất cả chiến lược vị trí. 

Trường hợp cạnh ẩn là khi tất cả các số đều là số chẵn. Khi đó mọi tổng đều là số chẵn nên chênh lệch luôn là số chẵn, nghĩa là Bob thắng bất kể chơi thế nào. Ngược lại, nếu có ít nhất một số lẻ thì tính chẵn lẻ của hiệu cuối cùng luôn là số lẻ, nghĩa là Alice thắng. Điều này không hiển nhiên nếu không giảm vấn đề về bảo toàn chẵn lẻ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng trò chơi. Ở mỗi bước, người chơi chọn một số có sẵn và lặp lại trên nhiều tập hợp còn lại, theo dõi cả hai tổng. Đây thực sự là một cây trò chơi trong đó mỗi trạng thái phân nhánh thành tất cả các phần tử còn lại. Số lượng trạng thái là giai thừa tính bằng n vì mỗi thứ tự chọn tương ứng với một chuỗi chơi có thể có. 

Ngay cả với tính năng ghi nhớ, không gian trạng thái vẫn theo cấp số nhân vì nhiều tập hợp còn lại có thể được biểu diễn thành 2^n tập hợp con và việc chuyển đổi phụ thuộc vào lượt của người chơi. Điều này làm cho nó hoàn toàn không khả thi đối với n đến 1e6. 

Quan sát quan trọng là thứ tự lựa chọn không ảnh hưởng đến nhiều tập hợp số mà mỗi người chơi kết thúc với lối chơi tối ưu, vì không có hạn chế nào về phần tử nào có thể được lấy. Mọi hoán vị của chuỗi đều tương ứng với một thứ tự chơi hợp lệ. Điều này có nghĩa là việc phân phối số cuối cùng giữa Alice và Bob không được kiểm soát một cách có ý nghĩa về mặt chiến lược. 

Thay vì suy nghĩ về lựa chọn tối ưu, chúng ta xem xét tính chẵn lẻ của tổng số tiền. Gọi S là tổng của tất cả các số. Vì Alice và Bob chia tất cả các phần tử nên chúng ta có S1 + S2 = S. Giá trị chúng ta quan tâm là |S1 − S2|, bằng |S − 2S2|. Modulo 2, điều này đơn giản hóa rất nhiều: 2S2 luôn chẵn, do đó tính chẵn lẻ của hiệu chính xác là tính chẵn lẻ của S. 

Vì vậy, toàn bộ trò chơi chỉ dừng lại ở việc kiểm tra xem tổng số tiền là số lẻ hay số chẵn. Nếu tổng là số lẻ thì hiệu số là số lẻ nên Alice thắng. Nếu tổng số chẵn thì Bob thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trò chơi Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Giảm chẵn lẻ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tổng của tất cả các phần tử trong dãy. Điều này hợp lệ vì mọi phần tử cuối cùng sẽ được lấy bởi chính xác một người chơi, do đó tổng được cố định bất kể thứ tự chơi. 
2. Xác định xem tổng số này là số lẻ hay số chẵn bằng cách kiểm tra bit có trọng số nhỏ nhất của nó. Chỉ tính chẵn lẻ mới quan trọng vì điều kiện thắng phụ thuộc vào việc chênh lệch tuyệt đối có phải là số lẻ hay không. 
3. Nếu tổng là số lẻ, xuất ra "Alice", nếu không thì xuất ra "Bob". Điều này xuất phát trực tiếp từ sự tương đương giữa tính chẵn lẻ của tổng số và tính chẵn lẻ của chênh lệch cuối cùng. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tổng tổng của tất cả các phần tử được giữ nguyên bất kể người chơi chọn phần tử như thế nào. Vì S1 + S2 = S luôn đúng nên biểu thức |S1 − S2| có thể được viết lại thành |S − 2S2|. Số hạng 2S2 luôn chẵn nên không ảnh hưởng đến tính chẵn lẻ. Do đó, tính chẵn lẻ của chênh lệch cuối cùng được cố định trước khi trận đấu bắt đầu và không phụ thuộc vào bất kỳ quyết định nào. Điều này loại bỏ hoàn toàn khía cạnh trò chơi và biến vấn đề thành một phép tính một lượt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    arr = list(map(int, input().split()))
    
    total = sum(arr)
    
    if total % 2 == 1:
        print("Alice")
    else:
        print("Bob")

if __name__ == "__main__":
    main()
```Việc thực hiện rất trực tiếp: chúng tôi tích lũy tổng của tất cả các số trong một lần chuyển. Việc sử dụng tính tổng tích hợp của Python đã đủ tuyến tính và hiệu quả cho n lên đến 1e6. Chúng tôi tránh mọi sự sắp xếp hoặc mô phỏng vì chúng không cần thiết. 

Chi tiết tinh tế duy nhất là đảm bảo không có logic trung gian nào cản trở tính chẵn lẻ. Chúng tôi không cần theo dõi lượt hoặc mô phỏng việc loại bỏ vì kết quả cuối cùng chỉ phụ thuộc vào toàn bộ nhiều tập hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```Chúng tôi tính toán tổng số tiền đang chạy. 

| Bước | Giá trị hiện tại | Tổng Chạy | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 3 | 
| 3 | 3 | 6 | 
| 4 | 4 | 10 | 
| 5 | 5 | 15 | 

Tổng số là 15, là số lẻ nên Alice thắng. Điều này cho thấy ngay cả với các phần tử chẵn lẻ hỗn hợp, chỉ có giá trị chẵn lẻ cuối cùng là quan trọng chứ không phải sự phân phối. 

### Ví dụ 2 

đầu vào:```
2
2 4
```| Bước | Giá trị hiện tại | Tổng Chạy | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 4 | 6 | 

Tổng số tiền là 6, chẵn nên Bob thắng. Điều này khẳng định rằng khi tất cả các phần tử đều chẵn, Alice không bao giờ có thể tạo ra một hiệu số lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tổng một lượt trên n phần tử | 
| Không gian | O(1) | Chỉ lưu trữ tổng số đang chạy | 

Các ràng buộc cho phép tối đa 1e6 phần tử, do đó quá trình quét tuyến tính vừa vặn thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input())
    arr = list(map(int, input().split()))
    total = sum(arr)
    return "Alice" if total % 2 == 1 else "Bob"

# provided samples
assert run("5\n1 2 3 4 5\n") == "Alice"
assert run("2\n2 4\n") == "Bob"

# minimum size
assert run("1\n1\n") == "Alice"
assert run("1\n2\n") == "Bob"

# all equal odd
assert run("4\n1 1 1 1\n") == "Bob"

# mixed parity
assert run("3\n1 2 2\n") == "Alice"

# large even-only
assert run("3\n1000000000 1000000000 2\n") == "Bob"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | Alice | trường hợp tổng lẻ nhỏ nhất | 
| 1 2 | Bob | trường hợp tổng chẵn nhỏ nhất | 
| 1 1 1 1 | Bob | nhiều tỷ lệ cược hủy thành chẵn | 
| 1 2 2 | Alice | tính chẵn lẻ hỗn hợp | 
| bộ chẵn lớn | Bob | ổn định giá trị lớn | 

## Vỏ cạnh 

Trường hợp một phần tử tối thiểu sẽ hiển thị quy tắc cốt lõi ngay lập tức. Đối với đầu vào`1 1`, Alice lấy phần tử duy nhất và hiệu là 1, là số lẻ nên Alice thắng. Thuật toán tính tổng = 1 và đưa ra kết quả chính xác là Alice. 

Đối với một phần tử chẵn như`1 2`, Bob thắng vì tổng là 2, chẵn. Alice lấy 2, Bob không nhận được gì, chênh lệch là 2, vẫn chẵn. Thuật toán trả về Bob một cách nhất quán. 

Trường hợp có nhiều số lẻ như`1 1 1 1`tạo ra tổng số tiền là 4. Mặc dù người chơi thay phiên nhau chọn, nhưng bất kỳ sự phân chia nào cũng dẫn đến sự chia chẵn lẻ bằng nhau và chênh lệch là chẵn. Thuật toán nắm bắt điều này một cách trực tiếp mà không cần mô phỏng bất kỳ động thái nào. 

Những ví dụ này xác nhận rằng không có cách chơi chiến lược nào làm thay đổi kết quả ngang bằng và mức giảm dựa trên tổng là đủ.
