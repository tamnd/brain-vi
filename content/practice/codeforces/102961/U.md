---
title: "CF 102961U - Máy móc nhà máy"
description: "Chúng ta được cấp một tập hợp các máy móc độc lập, mỗi máy có thể liên tục sản xuất ra những sản phẩm giống hệt nhau. Máy thứ i sản xuất một mặt hàng trong khoảng thời gian cố định, vì vậy nếu nó chạy trong tổng thời gian T, nó sẽ đóng góp khoảng T chia cho thời gian xử lý, làm tròn xuống…"
date: "2026-07-04T06:56:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "U"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 43
verified: true
draft: false
---

[CF 102961U - Máy móc trong nhà máy](https://codeforces.com/problemset/problem/102961/U) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các máy móc độc lập, mỗi máy có thể liên tục sản xuất ra những sản phẩm giống hệt nhau. Máy thứ i sản xuất một mặt hàng trong khoảng thời gian cố định, vì vậy nếu nó chạy trong tổng thời gian T, nó sẽ đóng góp khoảng T chia cho thời gian xử lý, làm tròn thành toàn bộ mặt hàng. 

Bên cạnh những chiếc máy này, còn có một số lượng mặt hàng mục tiêu mà nhà máy phải sản xuất. Tất cả các máy đều hoạt động song song và đầu ra của chúng được tích lũy. Nhiệm vụ là xác định lượng thời gian cần thiết nhỏ nhất để tổng số mặt hàng được sản xuất trên tất cả các máy ít nhất là mục tiêu yêu cầu. 

Đầu vào bao gồm số lượng máy, yêu cầu sản xuất và danh sách tốc độ máy. Mỗi tốc độ cho biết máy đó mất bao lâu để sản xuất một mặt hàng. Đầu ra là một số nguyên biểu thị thời gian tối thiểu mà yêu cầu sản xuất được đáp ứng. 

Cấu trúc ràng buộc thường bao hàm tối đa khoảng 10^5 máy và giá trị mục tiêu lớn, thường lên tới 10^18 hoặc cường độ tương tự. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào nâng cao thời gian từng bước, vì ngay cả việc lặp lại theo đơn vị thời gian cũng sẽ vượt xa giới hạn khả thi. Ngay cả mô phỏng trên mỗi đơn vị cũng sẽ bùng nổ tới 10^18 hoạt động trong trường hợp xấu nhất và thậm chí mô phỏng theo mỗi sự kiện vẫn sẽ quá chậm vì các sự kiện trên tất cả các máy đan xen dày đặc. 

Một số trường hợp đặc biệt có xu hướng phá vỡ lý luận ngây thơ. Nếu có một máy rất nhanh trong số nhiều máy chậm, thì bất kỳ cách tiếp cận nào phân phối công việc một cách tham lam trên mỗi máy mà không tổng hợp theo thời gian sẽ đánh giá thấp sự đóng góp. Ví dụ: nếu thời gian của máy là [1, 100, 100] và mục tiêu là 5 thì câu trả lời đúng là 5, vì chỉ riêng máy đầu tiên đã hoàn thành mọi thứ. Phân phối tham lam bị lỗi cố gắng "gán các mục" cho máy theo thứ tự mà không lập mô hình thời gian song song sẽ phân bổ tải không chính xác. 

Một trường hợp tinh tế khác xuất hiện khi tất cả các máy đều chậm nhưng mục tiêu lại nhỏ. Ví dụ: các máy [10, 10, 10] có mục tiêu 1 sẽ trả về 10. Bất kỳ cách tiếp cận nào cố gắng tính thời gian xử lý trung bình một cách nhầm lẫn sẽ đề xuất sai thời gian nhỏ hơn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng thời gian bắt đầu từ 0 và tăng dần từng bước. Tại mỗi giá trị thời gian, chúng tôi tính toán mỗi máy đã sản xuất được bao nhiêu mặt hàng bằng cách chia thời gian đã trôi qua cho thời gian xử lý và tính tổng giữa các máy. Khi tổng số đạt mục tiêu, chúng tôi sẽ trả về thời gian hiện tại. 

Cách tiếp cận này đúng vì nó phản ánh trực tiếp định nghĩa về sản xuất theo thời gian. Mọi máy đều đóng góp chính xác các mục tầng (T / a_i) tại thời điểm T, do đó việc kiểm tra từng bước thời gian sẽ đảm bảo tính chính xác. 

Điểm thất bại là sự tăng trưởng của trục thời gian. Nếu thời gian máy và kích thước mục tiêu lớn thì câu trả lời có thể lên tới 10^18. Ngay cả một lần vượt qua phạm vi này là không thể. Việc tính toán sản xuất ở mỗi bước cũng tốn O(n), do đó độ phức tạp tổng thể trở thành O(n · câu trả lời), không thể sử dụng được. 

Quan sát quan trọng là tổng số vật phẩm được tạo ra tại thời điểm T là đơn điệu trong T. Nếu chúng ta có đủ vật phẩm tại thời điểm T thì thời gian lớn hơn cũng sẽ đáp ứng yêu cầu. Cấu trúc đơn điệu này cho phép chúng ta thay thế tìm kiếm tuyến tính theo thời gian bằng tìm kiếm nhị phân. 

Thay vì mô phỏng từng khoảnh khắc, chúng ta đặt ra một câu hỏi khả thi: với một thời gian cố định T, liệu tất cả các máy có thể cùng nhau sản xuất ra ít nhất k vật phẩm không? Việc kiểm tra này diễn ra nhanh chóng vì chỉ yêu cầu tính tổng tầng(T/a_i) trên tất cả các máy. Với vị từ này, chúng ta tìm kiếm nhị phân T nhỏ nhất thỏa mãn nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · T) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân đúng giờ | O(n log T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng ta rút gọn bài toán về việc tìm thời gian T tối thiểu để hàm sản xuất đạt được mục tiêu. 

1. Xác định hàm tính toán số lượng mặt hàng được sản xuất bởi tất cả các máy trong một thời gian T nhất định bằng cách tính tổng T // a_i trên tất cả các máy. Hàm này mô hình hóa toàn bộ sản lượng của nhà máy tại một thời điểm cố định. 
2. Chọn phạm vi tìm kiếm theo thời gian. Giới hạn dưới bằng 0 và giới hạn trên có thể là kịch bản chậm nhất có thể xảy ra một cách an toàn khi chỉ một máy tạo ra tất cả các mục, thường được đặt là max(a_i) * k. 
3. Thực hiện tìm kiếm nhị phân trong khoảng thời gian này. Tại mỗi điểm giữa T, hãy tính tổng sản lượng bằng hàm ở bước 1. 
4. Nếu việc sản xuất tại T ít nhất là mục tiêu, hãy ghi T làm câu trả lời ứng viên và chuyển tìm kiếm sang trái vì chúng tôi cố gắng giảm thiểu thời gian. 
5. Ngược lại, di chuyển tìm kiếm sang phải vì T quá nhỏ để đáp ứng yêu cầu sản xuất. 
6. Tiếp tục cho đến khi hết không gian tìm kiếm, trả về T nhỏ nhất có thể. 

Sự lựa chọn thiết kế quan trọng là chức năng kiểm tra được tính toán lại từ đầu mỗi lần. Điều này có thể chấp nhận được vì tính toán tuyến tính theo số lượng máy, trong khi độ sâu tìm kiếm là logarit trong phạm vi câu trả lời. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào tính đơn điệu của hàm sản xuất. Đối với bất kỳ bộ tốc độ máy cố định nào, việc tăng T chỉ có thể tăng hoặc duy trì số lượng mặt hàng được sản xuất chứ không bao giờ giảm. Điều này đảm bảo rằng điều kiện khả thi tạo thành một khoảng liền kề trên trục số: tất cả thời gian dưới một ngưỡng nhất định đều không hợp lệ và tất cả thời gian trên ngưỡng đó đều hợp lệ. Do đó, tìm kiếm nhị phân được đảm bảo hội tụ về thời gian hợp lệ nhỏ nhất mà không bỏ qua ranh giới thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(machines, t, k):
    total = 0
    for a in machines:
        total += t // a
        if total >= k:
            return True
    return False

def solve():
    n, k = map(int, input().split())
    machines = list(map(int, input().split()))

    lo, hi = 0, min(machines) * k
    ans = hi

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(machines, mid, k):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Cốt lõi của giải pháp là`can`chức năng đánh giá liệu thời gian ứng viên có đủ hay không. Nó tính tổng các khoản đóng góp từ mỗi máy bằng cách sử dụng phép chia số nguyên, mã hóa trực tiếp số lượng mục đầy đủ mà mỗi máy hoàn thành theo thời gian`t`. 

Tìm kiếm nhị phân duy trì một cửa sổ thu nhỏ các câu trả lời có thể có. Bất cứ khi nào điểm giữa khả thi, nó sẽ được lưu trữ và việc tìm kiếm tiếp tục sang trái để tìm thời gian hợp lệ nhỏ hơn. Giới hạn trên được chọn là`min(machines) * k`bởi vì ngay cả chiếc máy nhanh nhất cũng có thể sản xuất tất cả các mặt hàng trong thời gian đó, khiến nó trở thành một ràng buộc an toàn trong trường hợp xấu nhất. 

Phải cẩn thận với tình trạng tràn trong các ngôn ngữ có kiểu số nguyên cố định, vì`t`Và`k`có thể lớn. Trong Python việc này được xử lý một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 7
3 2 5
```Chúng tôi muốn 7 mặt hàng, máy sản xuất cứ sau 3, 2 và 5 đơn vị. 

| giữa thời gian T | T//3 | T//2 | T//5 | tổng cộng | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 1 | 2 | 1 | 4 | không | 
| 10 | 3 | 5 | 2 | 10 | vâng | 
| 7 | 2 | 3 | 1 | 6 | không | 
| 8 | 2 | 4 | 1 | 7 | vâng | 

Tìm kiếm nhị phân thu hẹp về 8 là thời gian nhỏ nhất tạo ra ít nhất 7 mục. Điều này chứng tỏ tính khả thi của sự chuyển đổi mạnh mẽ từ sai sang đúng như thế nào. 

### Ví dụ 2 

đầu vào:```
2 1
10 10
```Chỉ cần một mục và cả hai máy đều chậm. 

| T | T//10 | T//10 | tổng cộng | khả thi | 
| --- | --- | --- | --- | --- | 
| 5 | 0 | 0 | 0 | không | 
| 10 | 1 | 1 | 2 | vâng | 

Câu trả lời là 10, cho thấy ngay cả nhiều máy cũng không giúp ích được gì nếu mục tiêu cực kỳ nhỏ và mỗi máy có tốc độ chậm giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(max(a_i) · k)) | Mỗi bước tìm kiếm nhị phân sẽ quét tất cả các máy và số bước là logarit trong phạm vi tìm kiếm | 
| Không gian | O(1) | Chỉ có một vài bộ đếm và mảng đầu vào được lưu trữ | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc điển hình lên tới 10^5 máy và giá trị mục tiêu lớn, vì khoảng 60 lần lặp tìm kiếm nhị phân là đủ ngay cả đối với phạm vi rất lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can(machines, t, k):
        total = 0
        for a in machines:
            total += t // a
            if total >= k:
                return True
        return False

    def solve():
        n, k = map(int, input().split())
        machines = list(map(int, input().split()))

        lo, hi = 0, min(machines) * k
        ans = hi

        while lo <= hi:
            mid = (lo + hi) // 2
            if can(machines, mid, k):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        return str(ans)

    return solve()

# custom cases
assert run("3 7\n3 2 5\n") == "8", "basic case"
assert run("2 1\n10 10\n") == "10", "small target"
assert run("1 5\n2\n") == "10", "single machine"
assert run("4 10\n1 2 3 4\n") == "6", "mixed speeds"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 7 / 3 2 5 | 8 | quá trình chuyển đổi tìm kiếm nhị phân tiêu chuẩn | 
| 2 1 / 10 10 | 10 | trường hợp cạnh mục tiêu tối thiểu | 
| 1 5/2 | 10 | độ chính xác của máy đơn | 
| 4 10 / 1 2 3 4 | 6 | tốc độ và tích lũy không đồng nhất | 

## Vỏ cạnh 

Khi chỉ có một máy, tìm kiếm nhị phân sẽ chuyển sang tìm bội số nhỏ nhất của thời gian xử lý để đạt được mục tiêu. Đối với đầu vào`1 5`với máy`[2]`, kiểm tra tính khả thi tăng từ 0 mục tại thời điểm 1 lên 5 mục tại thời điểm 10 và thuật toán hội tụ chính xác đến 10 vì chỉ có chu kỳ sản xuất đầy đủ mới quan trọng. 

Khi tất cả các máy đều giống hệt nhau, giải pháp phụ thuộc hoàn toàn vào phép nhân số lượng hơn là phân phối. Vì`[5, 5, 5]`và mục tiêu 3, thời điểm 5 đã tạo ra 3 mục và bất kỳ nỗ lực ngây thơ nào nhằm phân công tuần tự các nhiệm vụ cho mỗi máy sẽ giả định không chính xác rằng cần có nhiều chu kỳ, trong khi mô hình chính xác sẽ tổng hợp các đóng góp trong một bước. 

Khi mục tiêu cực kỳ lớn, chẳng hạn như gần 10^18, chỉ tìm kiếm nhị phân vẫn khả thi. Thuật toán không bao giờ lặp lại theo thời gian một cách rõ ràng và mỗi bước vẫn là tổng giới hạn trên các máy, đảm bảo tính chính xác ngay cả ở quy mô cực lớn.
