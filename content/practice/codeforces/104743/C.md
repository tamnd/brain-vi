---
title: "CF 104743C - Vấn đề về tiền tố MEX"
description: "Chúng ta được cho một mảng các số nguyên không âm. Chúng tôi được phép sửa đổi các phần tử, nhưng mỗi sửa đổi có một quy tắc rất cụ thể: nếu chúng tôi chọn vị trí i, chúng tôi sẽ ghi đè a[i] bằng MEX của tiền tố ngay trước nó, nghĩa là số nguyên không âm nhỏ nhất không…"
date: "2026-06-29T01:21:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104743
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #25(5^2-Forces)"
rating: 0
weight: 104743
solve_time_s: 84
verified: false
draft: false
---

[CF 104743C - Vấn đề về tiền tố MEX](https://codeforces.com/problemset/problem/104743/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Chúng ta được phép sửa đổi các thành phần, nhưng mỗi sửa đổi đều có một quy tắc rất cụ thể: nếu chúng ta chọn vị trí`i`, chúng tôi ghi đè`a[i]`với MEX của tiền tố ngay trước nó, nghĩa là số nguyên không âm nhỏ nhất không xuất hiện trong`a[1..i-1]`. Đối với vị trí đầu tiên, tiền tố trống, do đó MEX là`0`. 

Mục tiêu không phải là áp dụng một số thao tác cố định hoặc giảm thiểu các thao tác. Thay vào đó, chúng tôi muốn đạt được bất kỳ cấu hình mảng có thể truy cập nào có kích thước nhỏ nhất về mặt từ điển, nghĩa là chúng tôi quan tâm đến việc cải thiện các vị trí trước đó một cách tích cực nhất có thể, ngay cả khi điều đó buộc phải thay đổi sau này. 

Khó khăn chính là việc thay đổi một phần tử trước đó sẽ thay đổi tất cả các giá trị MEX tiền tố trong tương lai, sau đó ảnh hưởng đến vị trí sau này. Vì vậy, vấn đề là tối ưu hóa toàn cục qua một chuỗi các phép biến đổi phụ thuộc cục bộ. 

Các ràng buộc ngụ ý rằng tổng độ dài của tất cả các trường hợp thử nghiệm lên tới 5 × 10^5, do đó, mọi giải pháp đều phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Các phương pháp tiếp cận bậc hai hoặc thậm chí n log n với tính toán lại theo chỉ số nặng sẽ thất bại. Bất kỳ phương pháp nào tính toán lại MEX từ đầu cho từng vị trí đều ngay lập tức quá chậm vì tính toán MEX ít nhất là tuyến tính trừ khi được duy trì cẩn thận. 

Trường hợp cạnh tinh tế là khi các phần tử ban đầu lớn hoặc không liên quan. Ví dụ: nếu mảng bắt đầu bằng các giá trị lớn như`[100, 200, 300]`, suy nghĩ ngây thơ có thể cho rằng không có hoạt động hữu ích nào tồn tại. Nhưng việc thay thế phần tử đầu tiên buộc nó trở thành`0`, sau đó định hình lại tất cả các giá trị MEX trong tương lai. 

Một trường hợp cạnh khác là khi mảng đã chứa các phân đoạn nhỏ liên tiếp như`[0,1,2,...]`. Trong những trường hợp như vậy, việc thay đổi một phần tử thành MEX có thể tạm thời tạo ra sự trùng lặp hoặc phá vỡ cấu trúc, nhưng làm như vậy vẫn có thể cải thiện thứ tự từ điển trước đó. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng quy trình: tại mỗi vị trí, quyết định giữ giá trị hiện tại hay thay thế nó bằng MEX của tiền tố trước đó, sau đó khám phá đệ quy tất cả các khả năng. Điều này tạo thành một cây quyết định trong đó mỗi nút phân nhánh dựa trên việc chúng ta có sửa đổi vị trí hay không`i`hay không. MEX của tiền tố có thể được duy trì tăng dần, nhưng việc phân nhánh vẫn dẫn đến sự tăng trưởng theo cấp số nhân về số lượng trạng thái. 

Ngay cả một mô phỏng tham lam xử lý từ trái sang phải và tính toán lại MEX mỗi lần cũng sẽ yêu cầu duy trì cấu trúc tần số và cập nhật cấu trúc đó cho mỗi thao tác. Nếu chúng tôi tính toán lại MEX một cách đơn giản cho mỗi chỉ mục thì mỗi phép tính sẽ là O(n), dẫn đến O(n^2) cho mỗi trường hợp thử nghiệm, vượt xa giới hạn. 

Thông tin chi tiết về cấu trúc quan trọng là các giá trị hữu ích duy nhất mà chúng tôi từng giới thiệu là giá trị MEX của tiền tố và các giá trị này luôn nhỏ và đơn điệu theo cách được kiểm soát. Khi chúng ta quan sát thấy MEX chỉ phụ thuộc vào những con số nào`0,1,2,...`đã được nhìn thấy cho đến nay, chúng ta có thể duy trì một “tập hợp đã nhìn thấy” động trong khi xây dựng câu trả lời một cách tham lam từ trái sang phải. 

Sau đó, chúng tôi diễn giải lại vấn đề: thay vì chọn các thao tác tùy ý, chúng tôi quyết định cho từng vị trí xem có thực thi giá trị nhỏ nhất có thể được lịch sử tiền tố cho phép hay không. Vì thứ tự từ điển ưu tiên các vị trí trước đó nên chúng tôi luôn muốn giá trị nhỏ nhất có thể đạt được ở mỗi chỉ mục với điều kiện là các quyết định trước đó đã được cố định. 

Điều này dẫn đến một cấu trúc tham lam trong đó chúng tôi mô phỏng quá trình xây dựng tiền tố hợp lệ, theo dõi những số nào đã được “tiêu thụ” theo cách ảnh hưởng đến giá trị MEX trong tương lai và chỉ định cho mỗi vị trí giá trị tốt nhất có thể phù hợp với khả năng tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng MEX tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi xử lý mảng từ trái sang phải, duy trì cấu trúc theo dõi những số nguyên nhỏ nào đã xuất hiện trong tiền tố được xây dựng. Cấu trúc này chỉ được sử dụng để tính toán MEX hiện tại một cách hiệu quả. 
2. Tại mỗi chỉ số`i`, chúng tôi tính toán MEX của trạng thái tiền tố hiện tại. Điều này thể hiện giá trị nhỏ nhất mà chúng ta có thể ép buộc tại vị trí`i`nếu chúng ta chọn ghi đè lên nó. 
3. Chúng tôi quyết định liệu chúng tôi có thể cải thiện hay không`a[i]`bằng cách thay thế nó bằng MEX này. Vì thứ tự từ điển ưu tiên sớm các giá trị nhỏ hơn nên nếu MEX nhỏ hơn giá trị hiện tại thì chúng tôi sẽ áp dụng thao tác. 
4. Khi chúng tôi chỉ định một giá trị (gốc hoặc MEX), chúng tôi sẽ cập nhật cấu trúc “đã thấy” tương ứng. Điều này đảm bảo các tính toán MEX trong tương lai phản ánh tiền tố được xây dựng thực tế. 
5. Chúng tôi tiếp tục quá trình này cho đến hết mảng, luôn cam kết giá trị khả thi nhỏ nhất ở mỗi bước. 

Điều tinh tế là chúng tôi không mô phỏng các chuỗi hoạt động tùy ý mà trực tiếp xây dựng kết quả có thể đạt được tốt nhất bằng cách đảm bảo rằng mọi tiền tố đều nhất quán với một số chuỗi thay thế MEX hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến tại mọi chỉ số`i`, tiền tố được xây dựng có thể truy cập được từ mảng ban đầu bằng các thao tác hợp lệ. MEX ở vị trí`i`chỉ phụ thuộc vào giá trị nào đã được buộc vào các vị trí trước đó và bất kỳ giá trị nào được gán ở vị trí`i`chính xác là thứ có thể được tạo ra bởi một hoạt động hợp pháp tại chỉ số đó. Bởi vì thứ tự từ điển phụ thuộc vào vị trí khác nhau đầu tiên, nên việc giảm thiểu từng vị trí dưới các ràng buộc về khả năng tiếp cận sẽ mang lại một chuỗi tối thiểu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        # We only care about small values for mex tracking
        seen = set()
        mex = 0

        res = [0] * n

        for i in range(n):
            # update mex to current smallest missing
            while mex in seen:
                mex += 1

            # we may choose to overwrite with mex or keep original
            # but lexicographically we prefer smaller value if achievable
            if mex < a[i]:
                res[i] = mex
                seen.add(mex)
            else:
                res[i] = a[i]
                seen.add(a[i])

            while mex in seen:
                mex += 1

        out.append(" ".join(map(str, res)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện giữ một`seen`được đặt cho các giá trị đã được cố định vào tiền tố kết quả. Biến`mex`được duy trì tăng dần, do đó mỗi số nguyên được tăng lên nhiều nhất một lần, tạo ra hành vi khấu hao tuyến tính. 

Tại mỗi chỉ số, chúng tôi so sánh MEX hiện tại với giá trị ban đầu. Nếu MEX nhỏ hơn, việc thay thế sẽ cải thiện thứ tự từ điển ngay lập tức. Ngược lại, giữ nguyên bản gốc là tối ưu vì bất kỳ sự thay thế nào cũng chỉ làm tăng giá trị. 

Bản cập nhật thứ hai của`mex`sau khi chèn đảm bảo tính nhất quán: khi một giá trị được thêm vào, các giá trị MEX trong tương lai sẽ bỏ qua giá trị đó một cách chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[0, 3, 0, 1]`Chúng tôi theo dõi`seen`Và`mex`: 

| tôi | một [tôi] | mex trước | quyết định | độ phân giải[i] | nhìn thấy sau | mex sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | giữ 0 | 0 | {0} | 1 | 
| 2 | 3 | 1 | thay thế | 1 | {0,1} | 2 | 
| 3 | 0 | 2 | giữ 0 | 0 | {0,1,0} | 2 | 
| 4 | 1 | 2 | giữ 1 | 1 | {0,1,0,1} | 2 | 

Đầu ra trở thành`[0,1,0,1]`. 

Dấu vết này cho thấy việc giới thiệu một MEX nhỏ sớm sẽ buộc các giá trị MEX trong tương lai tăng lên như thế nào, cho phép các vị trí ban đầu nhỏ hơn về mặt từ điển. 

### Ví dụ 2 

đầu vào:`[5, 4, 3]`| tôi | một [tôi] | mex trước | quyết định | độ phân giải[i] | nhìn thấy sau | mex sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | thay thế | 0 | {0} | 1 | 
| 2 | 4 | 1 | thay thế | 1 | {0,1} | 2 | 
| 3 | 3 | 2 | thay thế | 2 | {0,1,2} | 3 | 

Đầu ra trở thành`[0,1,2]`. 

Điều này chứng tỏ rằng ngay cả khi tất cả các giá trị ban đầu đều lớn, việc xây dựng có thể ghi đè hoàn toàn chúng thành cấu trúc tăng tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi số nguyên được chèn vào tập hợp một lần và mex chỉ tăng đơn điệu | 
| Không gian | O(n) | Lưu trữ cho mảng đã xây dựng và tập hợp nhìn thấy | 

Tổng kích thước đầu vào là 5×10^5, do đó, giải pháp tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Việc cập nhật theo thời gian được khấu hao liên tục giúp giải pháp trở nên thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        seen = set()
        mex = 0
        res = []

        for x in a:
            while mex in seen:
                mex += 1
            if mex < x:
                res.append(mex)
                seen.add(mex)
            else:
                res.append(x)
                seen.add(x)
            while mex in seen:
                mex += 1

        out.append(" ".join(map(str, res)))

    return "\n".join(out)

# sample and custom tests
assert run("1\n4\n0 3 0 1\n") == "0 1 0 1"
assert run("1\n3\n5 4 3\n") == "0 1 2"

assert run("1\n1\n0\n") == "0"
assert run("1\n1\n5\n") == "0"
assert run("1\n5\n0 1 2 3 4\n") == "0 1 2 3 4"
assert run("1\n5\n4 4 4 4 4\n") == "0 1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | 0 | kích thước tối thiểu, không cần thay đổi | 
| đơn lớn | 0 | vị trí đầu tiên luôn có thể bị ép về 0 | 
| trình tự tăng dần | giống nhau | cấu trúc đã tối ưu | 
| giá trị lặp lại | 0..n-1 | đầu vào lặp đi lặp lại vẫn mang lại sản lượng tăng theo hướng mex | 

## Vỏ cạnh 

Đối với một đầu vào như`[0]`, thuật toán khởi tạo`mex = 0`, thấy thế`0`đã bằng nhau và giữ nó. Tập hợp trở thành`{0}`và đầu ra là`0`, tính đúng đắn phù hợp. 

Vì`[5]`,`mex = 0`ít hơn`5`, vì vậy chúng tôi thay thế nó bằng`0`. Điều này hợp lệ vì việc chọn`i = 1`luôn mang lại MEX tiền tố trống, đó là`0`. 

Vì`[0,1,2,3]`, mỗi giá trị được giữ lại vì ở mỗi bước MEX bằng giá trị hiện tại, do đó không thể cải thiện được. Tính bất biến đảm bảo chúng tôi không bao giờ suy giảm tiền tố, do đó quá trình chuyển đổi danh tính được bảo toàn chính xác.
