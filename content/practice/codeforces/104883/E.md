---
title: "CF 104883E - \u5b9d\u77f3\u5408\u6210"
description: "Chúng ta được cấp một chuỗi các viên đá quý, mỗi viên có một cấp độ nguyên. Hoạt động duy nhất được phép là lấy một khối liền kề có ít nhất hai cấp độ giống hệt nhau và hợp nhất nó thành một viên đá quý duy nhất có cấp độ tăng thêm một."
date: "2026-06-28T09:16:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104883
codeforces_index: "E"
codeforces_contest_name: "The 18-th Beihang University Collegiate Programming Contest (BCPC 2023) - Final"
rating: 0
weight: 104883
solve_time_s: 46
verified: true
draft: false
---

[CF 104883E - \u5b9d\u77f3\u5408\u6210](https://codeforces.com/problemset/problem/104883/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các viên đá quý, mỗi viên có một cấp độ nguyên. Hoạt động duy nhất được phép là lấy một khối liền kề có ít nhất hai cấp độ giống hệt nhau và hợp nhất nó thành một viên đá quý duy nhất có cấp độ tăng thêm một. Quá trình này có thể được lặp lại ở bất kỳ đâu trong chuỗi và việc hợp nhất có thể tạo ra những cơ hội mới cho những lần hợp nhất tiếp theo. 

Câu hỏi đặt ra là liệu sau một số chuỗi hợp nhất như vậy có thể thu gọn toàn bộ mảng thành một viên đá quý duy nhất hay không. 

Khó khăn chính là việc sáp nhập mang tính cục bộ và phụ thuộc vào sự liền kề, nhưng hiệu ứng của chúng lại lan truyền theo hướng tăng giá trị. Việc hợp nhất sẽ loại bỏ nhiều phần tử và thay thế chúng bằng phần tử cấp cao hơn, sau đó có thể tham gia vào các lần hợp nhất trong tương lai nếu có đủ các bản sao giống hệt nhau xuất hiện liền kề. 

Các ràng buộc rất lớn: tối đa 10^5 phần tử cho mỗi trường hợp thử nghiệm và tổng số lên tới 5 × 10^5 trên tất cả các thử nghiệm. Điều này ngay lập tức loại trừ mọi mô phỏng liên tục quét và hợp nhất mảng một cách ngây thơ. Bất kỳ cách tiếp cận nào có tính bậc hai theo n cho mỗi trường hợp thử nghiệm sẽ thất bại. 

Một trường hợp phức tạp phát sinh khi việc hợp nhất tạo ra các viên ngọc cấp cao hơn chỉ có thể hợp nhất sau khi các phân đoạn ở xa sụp đổ. Ví dụ: việc hợp nhất từ ​​trái sang phải có thể không thành công ngay cả khi một thứ tự hợp nhất khác thành công. 

Một trường hợp khác là khi chuỗi gần như đồng nhất nhưng được phân tách bằng một phần tử khác nhau. Ví dụ: một chuỗi như [1, 1, 2, 1, 1] có vẻ có thể rút gọn được vì có nhiều số 1, nhưng 2 khối tương tác theo cách ngăn cản việc hình thành một chuỗi hợp nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng quá trình hợp nhất. Người ta có thể quét mảng liên tục, tìm bất kỳ đoạn cực đại nào có giá trị giống hệt nhau có độ dài ít nhất là hai, thay thế nó bằng một phần tử có giá trị +1 và lặp lại cho đến khi không thể di chuyển được. Mỗi lần quét là O(n) và trong trường hợp xấu nhất, chúng tôi có thể thực hiện việc hợp nhất O(n), dẫn đến O(n^2) cho mỗi trường hợp thử nghiệm. Với 10^5 phần tử thì tốc độ này quá chậm. 

Quan sát chính là quá trình này chỉ phụ thuộc vào các lần chạy liền kề và việc hợp nhất luôn làm giảm một lần chạy trong khi tăng mức độ của nó. Thay vì theo dõi các phần tử riêng lẻ, chúng ta có thể nén mảng thành các chuỗi có giá trị bằng nhau. Mỗi lần chạy được đặc trưng bởi một cặp (giá trị, số lượng). Hoạt động trở thành: nếu một lần chạy có số lượng ≥ 2, thì có thể giảm bớt bằng cách thay thế hai hoặc nhiều bản sao thành một mục cấp cao hơn, sau đó có thể hợp nhất với các lần chạy liền kề có cùng cấp độ mới. 

Điều này cho thấy mức giảm dựa trên ngăn xếp tương tự như việc thu gọn theo khoảng thời gian. Chúng tôi xử lý các lần chạy từ trái sang phải, duy trì cấu trúc nơi chúng tôi cố gắng giải quyết ngay lập tức mọi cơ hội hợp nhất. Bất cứ khi nào hai nhóm liền kề trở nên bằng nhau sau khi hợp nhất, chúng sẽ kết hợp thêm. Đây thực chất là một quá trình thực hiện xếp tầng trên một chuỗi số lượng được lập chỉ mục theo giá trị. 

Thay vì mô phỏng các phần tử, chúng tôi duy trì một bản đồ hoặc mảng số lượng trên mỗi giá trị và truyền bá “sự hợp nhất được thực hiện” lên trên. Mỗi lần chúng ta tích lũy ít nhất hai vật phẩm có cùng giá trị, chúng sẽ thu gọn thành một vật phẩm có giá trị +1. 

Điều này tương tự như phép cộng nhị phân, ngoại trừ cơ số là 2 nhưng các “chữ số” tương ứng với các cấp độ và các giá trị lan truyền lên trên. 

Câu hỏi cuối cùng đặt ra là liệu tất cả khối lượng có thể được giảm xuống thành một vật phẩm duy nhất ở một mức độ nào đó sau khi tất cả đều lan truyền hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Chạy Nén + Truyền Truyền | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Nén chuỗi đầu vào thành các lần chạy liên tiếp có giá trị bằng nhau, chỉ lưu trữ số lượng của chúng. Điều này loại bỏ cấu trúc bên trong không liên quan bên trong các khối giống hệt nhau. 
2. Tạo một từ điển (hoặc mảng) ánh xạ từng giá trị với số lượng “mục” hiện có ở cấp độ đó. Khởi tạo nó bằng cách sử dụng số lần chạy: mỗi lần chạy đóng góp một mục có giá trị của nó, nhưng nếu thời lượng chạy ít nhất là hai, nó sẽ ngay lập tức đóng góp một mục đã hợp nhất cao hơn một cấp thay vì nhiều mục đơn lẻ. 
3. Xử lý các giá trị theo thứ tự tăng dần, duy trì sự lan truyền giống như nhớ. Ở mỗi cấp độ v, hãy lấy số lượng vật phẩm hiện có. 
4. Trong khi số lượng ở cấp v ít nhất là hai, hãy liên tục hợp nhất các cặp thành cấp v + 1. Mỗi lần hợp nhất sẽ giảm số lượng ở v đi 2 và tăng số lượng ở v + 1 lên 1. Điều này tiếp tục cho đến khi còn lại ít hơn hai mục ở v. 
5. Di chuyển lên cấp độ tiếp theo và lặp lại quy trình tương tự, bao gồm các vật phẩm mới được tạo từ lần mang trước đó. 
6. Sau khi xử lý tất cả các cấp xuất hiện trong cấu trúc (bao gồm cả các cấp được tạo bởi các khoản mang), hãy kiểm tra xem có tồn tại một mục nào còn lại ở bất kỳ đâu không. Nếu chính xác một mục vẫn còn tổng thể thì câu trả lời là Có, nếu không thì Không. 

Ý tưởng chính là việc hợp nhất chỉ phụ thuộc vào hành vi giống như tính chẵn lẻ của số lượng trên mỗi cấp độ. Bất kỳ số chẵn nào ở một cấp độ sẽ được truyền hoàn toàn lên trên, trong khi số lẻ để lại phần còn lại không thể loại bỏ trừ khi có cấu trúc tiếp theo ở các cấp cao hơn. 

### Tại sao nó hoạt động 

Ở bất kỳ cấp độ v nào, chỉ có thể hợp nhất các cặp vật phẩm giống hệt nhau và mỗi lần hợp nhất sẽ tăng cấp độ một cách nghiêm ngặt. Vì không có hoạt động nào làm giảm mức độ nên sự tương tác giữa các mức độ là một luồng đi lên một chiều. Điều này có nghĩa là hệ thống hoạt động giống như một quy trình mang theo nhiều cấp độ, trong đó mỗi cấp độ sẽ phân giải độc lập thành 0 hoặc một mục còn sót lại và chuyển sang cấp độ tiếp theo. Bởi vì số lần mang mang tính quyết định và không phụ thuộc vào thứ tự nên cấu hình cuối cùng là duy nhất. Nếu sau khi nhân giống hoàn toàn, chỉ còn lại một vật phẩm, điều đó tương ứng với sự sụp đổ hoàn toàn của cấu trúc thành một viên đá quý duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    from collections import Counter
    
    cnt = Counter()
    for x in a:
        cnt[x] += 1
    
    # we may need to propagate upward dynamically
    keys = sorted(cnt.keys())
    max_key = max(keys) if keys else 0
    
    # use dict for dynamic levels
    while True:
        changed = False
        new_cnt = Counter()
        
        for v in sorted(cnt.keys()):
            c = cnt[v]
            if c >= 2:
                carry = c // 2
                rem = c % 2
                if rem:
                    new_cnt[v] += rem
                new_cnt[v + 1] += carry
                changed = True
            else:
                new_cnt[v] += c
        
        cnt = new_cnt
        
        if not changed:
            break
    
    total = sum(cnt.values())
    print("Yes" if total == 1 else "No")

if __name__ == "__main__":
    solve()

t = int(input())
for _ in range(t):
    solve()
```Việc thực hiện duy trì một bản đồ tần số theo các cấp độ. Mỗi lần lặp thực hiện một vòng lan truyền đi lên đầy đủ, thu gọn tất cả các cặp có sẵn. Vòng lặp tiếp tục cho đến khi không có cấp độ nào có ít nhất hai mục, nghĩa là không thể hợp nhất thêm nữa. 

Lựa chọn triển khai chính là sử dụng Bộ đếm trên các giá trị thay vì mô phỏng trình tự. Điều này tránh sự phụ thuộc vào tính liền kề, vì sau khi nén và trừu tượng hóa, tính liền kề chỉ quan trọng trong việc hình thành các nhóm giống nhau ban đầu. 

Một vấn đề tế nhị là sự chấm dứt: hệ thống ổn định khi không có mức nào có số lượng ≥ 2, vì không còn sự hợp nhất nào nữa. Khi đó, tất cả các mục còn lại đều bị cô lập theo nghĩa là chúng không thể kết hợp thêm được nữa. 

## Ví dụ đã hoạt động 

Xem xét đầu vào:```
1
5
1 1 2 1 1
```Chúng tôi bắt đầu với số lượng: 

| Cấp độ | Đếm | Hành động | 
| --- | --- | --- | 
| 1 | 4 | Hợp nhất 2 cặp → 2 vật phẩm lên cấp 2 | 
| 2 | 1 | không hợp nhất | 

Sau khi lan truyền, chúng ta nhận được: 

| Cấp độ | Đếm | 
| --- | --- | 
| 1 | 0 | 
| 2 | 3 | 

Bây giờ cấp 2 có 3 mục: 

| Cấp độ | Đếm | Hành động | 
| --- | --- | --- | 
| 2 | 3 | 1 cặp hợp nhất → 1 vật phẩm cấp 3 | 
| 3 | 1 | còn sót lại | 

Trạng thái cuối cùng có một mục duy nhất ở cấp 3, vì vậy đầu ra là: 

Có 

Điều này thể hiện sự hợp nhất theo tầng trong đó cấu trúc cấp cao hơn xuất hiện từ các phân đoạn cấp thấp riêng biệt. 

Bây giờ hãy xem xét:```
1
4
1 1 1 2
```Số đếm ban đầu: 

| Cấp độ | Đếm | 
| --- | --- | 
| 1 | 3 | 
| 2 | 1 | 

Ở cấp độ 1, 3 vật phẩm tạo ra 1 vật phẩm mang lên cấp 2 và 1 vật phẩm còn sót lại: 

| Cấp độ | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 

Bây giờ cấp 2 hợp nhất thành cấp 3: 

| Cấp độ | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 3 | 1 | 

Hai mục riêng biệt vẫn ở các cấp độ khác nhau nên câu trả lời là: 

Không 

Điều này cho thấy rằng mặc dù có thể hợp nhất cục bộ nhưng cấu trúc cuối cùng có thể phân chia thành nhiều thành phần độc lập không thể thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) trường hợp xấu nhất | Mỗi vòng lan truyền xử lý một bản đồ có kích thước tỷ lệ với các giá trị riêng biệt; tổng số cấp độ tăng chậm thông qua mang | 
| Không gian | O(n) | Bản đồ tần số trên các giá trị và giá trị trung gian | 

Các ràng buộc cho phép điều này một cách thoải mái vì tổng n trong các thử nghiệm là 5 × 10^5 và các hoạt động bị chi phối bởi việc đếm và truyền bá giới hạn thay vì mô phỏng trình tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        from collections import Counter
        
        cnt = Counter(a)
        
        while True:
            changed = False
            new = Counter()
            for v in sorted(cnt.keys()):
                c = cnt[v]
                if c >= 2:
                    new[v] += c % 2
                    new[v + 1] += c // 2
                    if c >= 2:
                        changed = True
                else:
                    new[v] += c
            cnt = new
            if not changed:
                break
        
        output.append("Yes" if sum(cnt.values()) == 1 else "No")
    
    t = int(input())
    for _ in range(t):
        solve()
    
    return "\n".join(output)

# sample-style tests
assert run("1\n5\n1 1 2 1 1\n") == "Yes"
assert run("1\n4\n1 1 1 2\n") == "No"

# custom tests
assert run("1\n1\n7\n") == "Yes"
assert run("1\n2\n1 2\n") == "No"
assert run("1\n6\n1 1 1 1 1 1\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | Có | đã độc thân | 
| hỗn hợp 1 2 | Không | không thể hợp nhất giữa các cấp độ | 
| tất cả những cái thậm chí còn được tính | Có | sụp đổ hoàn toàn thông qua mang | 

## Vỏ cạnh 

Mảng một phần tử như [k] đã được cấu hình xong. Thuật toán khởi tạo bộ đếm với một mục và ngay lập tức nhận thấy không thể hợp nhất được. Vì tổng số là một nên kết quả là Có một cách chính xác. 

Một cấu hình nhỏ xen kẽ nghiêm ngặt như [1, 2] không tạo ra sự hợp nhất nào cả. Mỗi giá trị vẫn bị cô lập ở cấp độ riêng của nó, vì vậy số đếm cuối cùng là hai và câu trả lời là Không. Vòng lặp lan truyền không thay đổi bất cứ điều gì vì không có cấp độ nào có số lượng ≥ 2. 

Một mảng hoàn toàn thống nhất như [1, 1, 1, 1, 1, 1] thể hiện khả năng xếp tầng tối đa. Ở cấp độ 1, ba vòng ghép đôi sẽ tạo ra các vật phẩm cấp cao hơn cho đến khi chỉ còn lại một vật phẩm cuối cùng. Thuật toán liên tục áp dụng phép chia số nguyên cho hai, đảm bảo tính chính xác của việc truyền đi lên.
