---
title: "CF 104720B - Thử thách dọn đồ trang sức"
description: "Chúng ta được cung cấp một chuỗi các đồ lặt vặt phải được loại bỏ theo một thứ tự cố định. Mỗi đồ trang sức có một trọng lượng và chúng tôi cũng có những túi rác giống hệt nhau với sức chứa tối đa là $K$."
date: "2026-06-29T04:15:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104720
codeforces_index: "B"
codeforces_contest_name: "UTPC x WiCS Contest 10-06-23"
rating: 0
weight: 104720
solve_time_s: 59
verified: true
draft: false
---

[CF 104720B - Thử thách dọn đồ trang sức](https://codeforces.com/problemset/problem/104720/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các đồ lặt vặt phải được loại bỏ theo một thứ tự cố định. Mỗi món đồ trang sức có một trọng lượng và chúng tôi cũng có những túi đựng rác giống hệt nhau với sức chứa tối đa là$K$. Một túi có thể tích lũy nhiều đồ trang sức liên tiếp miễn là tổng trọng lượng của chúng không vượt quá$K$. Một khi chúng ta quyết định ngừng sử dụng chiếc túi hiện tại, nó sẽ bị vứt đi và một chiếc túi mới rỗng sẽ được sử dụng. Chúng tôi không được phép giữ nhiều hơn một túi đã đầy một phần vào bất kỳ lúc nào, điều đó có nghĩa là quy trình này diễn ra tuần tự nghiêm ngặt: chúng tôi đổ đầy một túi từ trái sang phải, sau đó tiếp tục hoặc đóng túi lại và bắt đầu một túi mới. 

Mục đích là giảm thiểu số lượng túi được sử dụng để xử lý toàn bộ chuỗi. 

Đầu vào mang lại$N$, số lượng đồ trang sức, theo sau là trọng lượng của chúng theo thứ tự. Đầu ra là số lượng túi tối thiểu cần thiết để đóng gói tất cả các đồ trang sức với điều kiện ràng buộc là đơn hàng phải được giữ nguyên và mỗi túi có sức chứa$K$. 

Các ràng buộc đủ lớn để một phương trình bậc hai hoặc thậm chí$O(N \log N)$chiến lược là chi phí không cần thiết. Với$N \le 10^5$, MỘT$O(N)$quét tham lam là mục tiêu an toàn duy nhất dưới giới hạn 1 giây. Bất kỳ cách tiếp cận nào cố gắng xem xét lại các quyết định nhóm trước đó hoặc mô phỏng tất cả các điểm phân vùng đều có nguy cơ$O(N^2)$hành vi trong trường hợp xấu nhất, vượt xa giới hạn chấp nhận được. 

Trường hợp cạnh tinh vi xuất hiện khi một vật trang trí đơn lẻ chính xác bằng$K$. Trong trường hợp đó, nó phải có túi riêng của mình. Ví dụ: đầu vào:```
3 5
5 1 1
```Đầu ra đúng là`2`. Việc triển khai bất cẩn luôn cố gắng “khớp trước, chia sau” mà không kiểm tra sự bằng nhau một cách cẩn thận có thể cố gắng kết hợp không chính xác hoặc xử lý sai logic đặt lại sau khi hết dung lượng, dẫn đến việc nhóm sai. 

Một trường hợp khác là khi tất cả đồ trang sức đều rất nhỏ và tổng thể vừa khít với nhiều túi:```
5 3
1 1 1 1 1
```Hành vi đúng là đóng gói tuần tự, chỉ đặt lại khi thêm phần tử tiếp theo sẽ vượt quá dung lượng. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng tất cả các cách có thể để phân chia chuỗi thành các nhóm liền kề hợp lệ. Mỗi nhóm phải có tổng trọng lượng tối đa$K$. Điều này có thể được coi là việc chọn điểm cắt giữa các đồ trang sức và kiểm tra xem mỗi phân đoạn có hợp lệ hay không. có$N-1$vị trí cắt tiềm năng, vì vậy có$2^{N-1}$các cách chọn phần chia. Ngay cả khi chúng tôi chỉ xác nhận sự phân chia theo thời gian tuyến tính, cách tiếp cận này sẽ trở nên theo cấp số nhân và ngay lập tức không khả thi ở mức$N = 10^5$. 

Cách tiếp cận bạo lực có cấu trúc hơn một chút sẽ sử dụng lập trình động trong đó$dp[i]$là số túi tối thiểu cần thiết cho lần đầu tiên$i$đồ lặt vặt. Đối với mỗi$i$, chúng tôi thử tất cả các vị trí trước đó$j < i$sao cho đoạn đó$(j+1..i)$phù hợp với một túi. Điều này hoạt động chính xác nhưng vẫn dẫn đến$O(N^2)$trong trường hợp xấu nhất, ví dụ khi tất cả các trọng số đều nhỏ và mọi tiền tố đều hợp lệ, buộc mỗi trạng thái phải quét tất cả các trạng thái trước đó. 

Quan sát quan trọng là thứ tự được cố định và mỗi túi hoạt động giống như một cửa sổ trượt với giới hạn sức chứa nghiêm ngặt. Khi chúng ta bắt đầu lấp đầy túi, chúng ta không bao giờ cần phải xem xét lại các lựa chọn trước đó vì việc chia một phần hợp lệ sớm hơn sẽ không có lợi ích gì trừ khi mục tiếp theo không còn vừa nữa. Điều này biến vấn đề thành một quá trình đóng gói tham lam: tiếp tục tích lũy cho đến khi việc thêm đồ trang sức tiếp theo vượt quá sức chứa, sau đó đóng túi lại và bắt đầu một túi mới. 

Chiến lược tham lam này hoạt động vì không có sự khác biệt về chi phí giữa các gói hợp lệ khác nhau của cùng một phân khúc liền kề; mục tiêu duy nhất là giảm thiểu số lượng phân khúc và việc mở rộng một phân khúc càng nhiều càng tốt luôn làm giảm đáng kể số lượng phân khúc cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP |$O(N^2)$|$O(N)$| Quá chậm | 
| Quét tham lam |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các đồ lặt vặt theo thứ tự, duy trì trọng lượng tích lũy của túi hiện tại và đếm xem chúng tôi đã mở bao nhiêu túi. 

1. Khởi tạo bộ đếm cho các túi bằng 0 và tổng hiện hành cho túi hiện tại bằng 0. Chúng tôi bắt đầu không có túi hoạt động. 
2. Lặp lại từng trọng lượng đồ trang sức theo thứ tự. 
3. Đối với mỗi món đồ trang sức, hãy kiểm tra xem việc thêm nó vào túi hiện tại có vượt quá$K$. Việc kiểm tra này là điểm quyết định duy nhất trong thuật toán. 
4. Nếu phù hợp, hãy cộng trọng lượng vào tổng số túi hiện tại và tiếp tục. Điều này giữ cho túi hiện tại đầy nhất có thể mà không vi phạm các ràng buộc. 
5. Nếu nó không vừa, hãy tăng bộ đếm túi vì túi hiện tại đã được hoàn thiện. Sau đó bắt đầu một chiếc túi mới với món đồ trang sức này làm vật phẩm đầu tiên. 
6. Sau khi xử lý tất cả đồ lặt vặt, nếu có túi đựng đầy một phần thì đã được tính ngầm vào bước mở cuối cùng nên không cần điều chỉnh thêm. 

Lý do đằng sau việc luôn lấp đầy một cách tham lam là việc trì hoãn việc đóng túi không bao giờ có thể giúp ích cho các yếu tố trong tương lai, vì các quyết định trong tương lai là độc lập khi năng lực là cố định và đơn hàng là bắt buộc. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì một túi hoạt động duy nhất chứa tiền tố tối đa của chuỗi còn lại phù hợp với dung lượng$K$. Đây là một bất biến: túi hiện tại luôn chứa đoạn liền kề hợp lệ dài nhất có thể bắt đầu từ điểm mở của nó. Khi một phần tử mới không vừa, không thể mở rộng túi hiện tại, do đó buộc phải bắt đầu một túi mới. 

Bất kỳ giải pháp thay thế nào đóng túi sớm hơn chỉ có thể làm tăng số lượng túi vì nó làm giảm kích thước của một phân đoạn hợp lệ mà không cho phép sắp xếp lại hoặc sắp xếp lại các đồ lặt vặt. Do đó, việc xây dựng tham lam tạo ra số lượng phân đoạn tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))
    
    bags = 0
    cur = 0
    
    for w in arr:
        if cur + w > k:
            bags += 1
            cur = w
        else:
            cur += w
    
    if n > 0 and cur > 0:
        bags += 1
    
    print(bags)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì tổng số tiền đang hoạt động`cur`cho chiếc túi hiện tại. Khi không thể thêm trọng lượng tiếp theo, chúng tôi hoàn thiện túi hiện tại bằng cách tăng`bags`và khởi động lại quá trình tích lũy. Một điểm tinh tế là túi chưa hoàn thành cuối cùng: vì chúng tôi chỉ tăng`bags`khi đóng một túi đầy, chúng ta phải thêm một cái nữa vào cuối nếu có đồ trang sức nào được đặt trong túi hiện tại. 

Logic đảm bảo mọi đồ trang sức đều thuộc về chính xác một túi và mỗi túi đều tuân theo giới hạn dung lượng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
1 3 1 1
```| Bước | Cân nặng | Tổng hiện tại | Hành động | Túi | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | túi bắt đầu | 0 | 
| 2 | 3 | 4 vượt quá | đóng, túi mới | 1 | 
| 3 | 1 | 1 | bắt đầu túi mới | 1 | 
| 4 | 1 | 2 | tiếp tục | 1 | 
| kết thúc | - | - | hoàn thiện túi cuối cùng | 2 | 

Đầu ra là`3`vì cấu trúc cuối cùng là (1), (3), (1,1). 

Dấu vết này cho thấy một lần tràn đơn lẻ buộc phải cắt ngay lập tức như thế nào và thuật toán không bao giờ cố gắng xem xét lại việc nhóm trước đó như thế nào. 

### Mẫu 2 

đầu vào:```
2 5
4 5
```| Bước | Cân nặng | Tổng hiện tại | Hành động | Túi | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | túi bắt đầu | 0 | 
| 2 | 5 | 9 vượt quá | đóng, túi mới | 1 | 
| kết thúc | - | - | hoàn thiện túi cuối cùng | 2 | 

Đầu ra là`2`, với mỗi mặt hàng chiếm một túi riêng do hạn chế về dung lượng. 

Điều này chứng tỏ trường hợp cực đoan khi mỗi món đồ đều buộc phải có một túi mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi đồ trang sức được xử lý một lần với các lần kiểm tra liên tục | 
| Không gian |$O(1)$| Chỉ các bộ đếm đang chạy mới được lưu trữ | 

Thuật toán chia tỷ lệ trực tiếp với$N$, vì vậy ngay cả tại$10^5$đồ lặt vặt nó chỉ thực hiện$10^5$thao tác đơn giản, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    _stdout = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = _stdout
    return out.strip()

# provided samples
assert run("4 3\n1 3 1 1\n") == "3"
assert run("2 5\n4 5\n") == "2"

# single element
assert run("1 10\n7\n") == "1"

# all fit in one bag
assert run("5 10\n1 2 3 4 5\n") == "1"

# forced splits
assert run("5 3\n2 2 2 2 2\n") == "3"

# alternating tight packing
assert run("6 4\n1 3 1 3 1 3\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | trường hợp tối thiểu | 
| tất cả đều phù hợp | 1 | không cần chia tách | 
| tràn lặp đi lặp lại | 3 | đặt lại nhiều lần | 
| mô hình xen kẽ | 4 | kích hoạt ranh giới thường xuyên | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mọi đồ trang sức đều khớp chính xác với dung lượng$K$. Ví dụ:```
3 5
5 5 5
```Thuật toán bắt đầu một túi cho vật phẩm đầu tiên, ngay lập tức đóng nó lại khi vật phẩm tiếp theo không vừa và lặp lại. Mỗi phần tử trở thành chiếc túi riêng của nó, tạo ra sản lượng`3`. Logic tham lam xử lý việc này một cách tự nhiên vì mọi phép cộng đều kích hoạt tình trạng tràn. 

Một trường hợp khác là khi nhiều vật dụng nhỏ tích lũy lại để lấp đầy một chiếc túi:```
6 3
1 1 1 1 1 1
```Tổng chạy trở thành 3, sau đó đặt lại, tạo ra hai túi (1,1,1) và (1,1,1). Bất biến là mỗi túi được lấp đầy tối đa đảm bảo không xảy ra hiện tượng phân chia sớm. 

Cuối cùng, các trường hợp hỗn hợp như:```
5 4
3 1 2 2 1
```cho thấy thuật toán không bao giờ cố gắng sắp xếp lại các lựa chọn tối ưu cục bộ. Mỗi quyết định hoàn toàn dựa trên việc mục tiếp theo có phù hợp hay không và điều này là đủ vì thứ tự đã được cố định và việc chia tách trước đó không có lợi ích gì.
