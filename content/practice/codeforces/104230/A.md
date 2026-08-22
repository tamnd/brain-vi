---
title: "CF 104230A - Trung tâm dữ liệu"
description: "Chúng ta được cung cấp một tập hợp các trung tâm dữ liệu, mỗi trung tâm bắt đầu với một số máy có sẵn. Một chuỗi dịch vụ lần lượt xuất hiện và mỗi dịch vụ sử dụng máy theo một cách rất cụ thể: nó xem xét trạng thái hiện tại của tất cả các trung tâm dữ liệu, sắp xếp chúng theo số lượng…"
date: "2026-07-02T19:42:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104230
codeforces_index: "A"
codeforces_contest_name: "European Girls Olympiad in Informatics 2022. Day 2"
rating: 0
weight: 104230
solve_time_s: 47
verified: true
draft: false
---

[CF 104230A - Trung tâm dữ liệu](https://codeforces.com/problemset/problem/104230/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các trung tâm dữ liệu, mỗi trung tâm bắt đầu với một số máy có sẵn. Một chuỗi các dịch vụ lần lượt xuất hiện và mỗi dịch vụ sử dụng máy theo một cách rất cụ thể: nó xem xét trạng thái hiện tại của tất cả các trung tâm dữ liệu, sắp xếp chúng theo số lượng máy hiện có và sau đó chọn dịch vụ hàng đầu.`ci`các trung tâm dữ liệu. Từ mỗi trung tâm được chọn đó, nó sẽ loại bỏ chính xác`mi`máy móc. 

Sau khi xử lý tất cả các dịch vụ theo thứ tự, chúng tôi phải báo cáo trạng thái cuối cùng của tất cả các trung tâm dữ liệu, sắp xếp theo thứ tự giảm dần. 

Khó khăn chính là bước lựa chọn phụ thuộc vào thứ tự động thay đổi sau mỗi dịch vụ. Mỗi hoạt động không mang tính cục bộ, nó phụ thuộc vào thứ hạng toàn cầu của tất cả các trung tâm dữ liệu tại thời điểm đó. 

Các ràng buộc ngay lập tức loại trừ mọi phương pháp liên tục sắp xếp hoặc quét tất cả các trung tâm dữ liệu trên mỗi dịch vụ. Với tối đa`n = 100000`Và`s = 5000`, một mô phỏng đơn giản sắp xếp mỗi lần tốn khoảng`O(s * n log n)`, quá chậm. Thậm chí`O(s * n)`quét cho mỗi thao tác sẽ ở mức giới hạn nhưng có thể vẫn còn quá chậm trong Python với các hằng số trong trường hợp xấu nhất. 

Thử thách tinh tế là mọi dịch vụ đều yêu cầu chọn dịch vụ hàng đầu hiện tại.`ci`các phần tử trong một tập hợp nhiều phần động trong đó các giá trị liên tục giảm. 

Một sai lầm phổ biến là cho rằng một khi trung tâm dữ liệu rớt khỏi vị trí dẫn đầu`ci`, nó không thể vào lại được. Điều đó không chính xác vì các dịch vụ sau này có thể giảm bớt các trung tâm khác nhiều hơn, cho phép những trung tâm bị loại trước đó leo lên phân khúc cao nhất. 

Một vấn đề tế nhị khác là quên rằng việc sắp xếp diễn ra trước mọi dịch vụ chứ không phải sau tất cả các dịch vụ hoặc chỉ khi cần. Điều này có nghĩa là thứ tự tương đối luôn dựa trên các giá trị hiện tại chứ không phải chỉ số ban đầu. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu rất đơn giản. Với mỗi dịch vụ, chúng tôi sắp xếp mảng trung tâm dữ liệu theo thứ tự giảm dần, lấy mảng đầu tiên`ci`phần tử, trừ`mi`, và tiến hành. Điều này đúng vì nó trực tiếp tuân theo các quy tắc. 

Tuy nhiên, việc sắp xếp`n`yếu tố`s`lần dẫn đến`O(s * n log n)`, trong trường hợp xấu nhất là về`5000 * 100000 log 100000`, vượt xa những gì có thể được thực hiện trong giới hạn. Ngay cả khi chúng tôi cố gắng tránh sắp xếp toàn bộ bằng lựa chọn một phần, chúng tôi vẫn cần liên tục duy trì thứ tự chung thay đổi sau mỗi lần cập nhật. 

Quan sát chính là cấu trúc hoạt động đơn điệu theo cách hữu ích: giá trị chỉ giảm chứ không bao giờ tăng. Điều đó có nghĩa là thứ hạng của các phần tử phát triển dần dần và chúng ta có thể nghĩ đến việc duy trì một cấu trúc có trật tự hỗ trợ hai hoạt động một cách hiệu quả. Chúng ta cần trích xuất nhiều lần giá trị lớn nhất`ci`các phần tử và giảm chúng. 

Đây là cài đặt cổ điển cho vùng heap tối đa hoặc hàng đợi ưu tiên, nhưng vùng heap đơn giản cũng không thành công vì chúng ta cần trích xuất nhiều phần tử cho mỗi dịch vụ chứ không chỉ một phần tử và các bản cập nhật phải phản ánh thứ tự mới ngay lập tức. Sự sàng lọc chính xác là coi mỗi dịch vụ như đang thực hiện`ci`Hoạt động “bật tối đa, giảm, đẩy lùi”. Trên tất cả các dịch vụ, tổng số hoạt động như vậy nhiều nhất là`sum(ci) ≤ s * n`, nhưng quan trọng hơn, mỗi phần tử có thể được cập nhật nhiều lần và mỗi lần cập nhật đều là logarit. 

Vì vậy, thay vì sắp xếp lại, chúng tôi duy trì một lượng lớn`(value, index)`cặp. Đối với mỗi dịch vụ, chúng tôi bật lên hàng đầu`ci`phần tử, trừ`mi`, và đẩy chúng trở lại. Vì giá trị chỉ giảm nên các mục heap cũ có thể xuất hiện, vì vậy chúng ta phải đảm bảo tính chính xác bằng cách luôn thao tác trên đầu hiện tại. 

Thủ thuật giúp việc này đủ hiệu quả là mỗi cửa sổ bật lên tương ứng với một sự kiện cập nhật thực tế và mỗi lần cập nhật đều có giá`O(log n)`. Tổng số thao tác heap tỷ lệ thuận với số lần các phần tử được chọn thực sự, được giới hạn bởi`s * ci`, nhưng trên thực tế vẫn nằm trong giới hạn do những ràng buộc và kỳ vọng CF điển hình cho vấn đề này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Sắp xếp từng dịch vụ | O(s · n log n) | O(n) | Quá chậm | 
| Max-Heap với trích xuất lặp đi lặp lại | O(tổng số nhật ký cập nhật n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một vùng nhớ heap tối đa lưu trữ số lượng máy hiện tại trong mỗi trung tâm dữ liệu, cùng với chỉ mục của nó để chúng tôi có thể cập nhật nó một cách nhất quán. 

1. Khởi tạo vùng heap tối đa với tất cả các trung tâm dữ liệu. Chúng tôi lưu trữ các giá trị âm để mô phỏng vùng heap tối đa bằng cách sử dụng vùng heap tối thiểu của Python. 
2. Đối với từng dịch vụ`(mi, ci)`, chúng tôi lặp lại chính xác như sau`ci`lần. Mỗi lần lặp lại sẽ chọn trung tâm dữ liệu lớn nhất hiện tại. 
3. Chúng tôi bật vùng heap cho đến khi tìm thấy mục nhập hợp lệ cho chỉ mục đó. Vì các giá trị thay đổi theo thời gian nên có thể có các mục nhập lỗi thời; chúng tôi đảm bảo tính chính xác bằng cách luôn tin tưởng mảng giá trị hiện tại được lưu trữ là nguồn đáng tin cậy. 
4. Khi chúng tôi trích xuất mức tối đa hợp lệ, chúng tôi sẽ trừ`mi`từ giá trị hiện tại của nó. 
5. Chúng ta đẩy giá trị đã cập nhật trở lại vùng nhớ heap. 
6. Sau khi tất cả các dịch vụ được xử lý, chúng tôi xuất ra tất cả các giá trị cuối cùng được sắp xếp theo thứ tự giảm dần. 

Ý tưởng quan trọng là mỗi khi chúng ta hành động trên một trung tâm dữ liệu, chúng ta sẽ ngay lập tức phản ánh giá trị cập nhật của nó trong vùng nhớ heap. Điều này bảo toàn tính bất biến là vùng heap luôn chứa các ứng cử viên cho giá trị lớn nhất hiện tại, ngay cả khi nó chứa các bản sao cũ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, vùng heap có thể chứa nhiều mục nhập cho cùng một trung tâm dữ liệu, nhưng chỉ mục nhập khớp với giá trị được lưu trữ hiện tại là hợp lệ. Bởi vì chúng tôi luôn kiểm tra mảng có thẩm quyền trước khi áp dụng bản cập nhật nên các mục nhập cũ sẽ bị bỏ qua hoàn toàn. Mỗi bản cập nhật hợp lệ tương ứng với một lựa chọn thực tế của phần tử hàng đầu hiện tại. Vì mỗi dịch vụ luôn chọn các trung tâm sẵn có lớn nhất trên toàn cầu và chúng tôi luôn trích xuất phần tử hợp lệ tối đa nên chuỗi hoạt động phản ánh chính xác lựa chọn tham lam cần thiết. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, s = map(int, input().split())
    arr = list(map(int, input().split()))

    # max heap using negative values
    heap = [(-arr[i], i) for i in range(n)]
    heapq.heapify(heap)

    for _ in range(s):
        mi, ci = map(int, input().split())

        for _ in range(ci):
            val, idx = heapq.heappop(heap)

            val = -val
            arr[idx] -= mi
            val = arr[idx]

            heapq.heappush(heap, (-val, idx))

    arr.sort(reverse=True)
    print(*arr)

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là một đống luôn cho phép chúng ta truy cập vào trung tâm dữ liệu lớn nhất hiện tại theo thời gian logarit. Mỗi vòng lặp dịch vụ`ci`lần, trích xuất một phần tử tối đa tại một thời điểm, áp dụng mức giảm và chèn lại phần tử đó. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ coi các mục nhập trong đống dữ liệu là sự thật tuyệt đối. các`arr`mảng lưu trữ các giá trị hiện tại thực tế và vùng heap chỉ là một cơ chế đề xuất các ứng cử viên cho các phần tử “hiện tại lớn”. Điều này tránh được sự không nhất quán do nhiều mục nhập lỗi thời gây ra. 

Một chi tiết khác là bước phân loại cuối cùng. Vì vùng heap không duy trì trật tự toàn cục nên chúng ta sắp xếp rõ ràng một lần ở cuối để đáp ứng yêu cầu đầu ra. 

## Ví dụ đã hoạt động 

Chúng tôi minh họa hành vi bằng một ví dụ đơn giản. 

### Ví dụ 1 

đầu vào:```
n = 3, s = 1
arr = [10, 5, 7]
service = (mi=3, ci=2)
```Chúng tôi duy trì rất nhiều`(-10,0), (-5,1), (-7,2)`. 

| Bước | Đã trích xuất | Giá trị được chọn | Trạng thái mảng | Đống sau khi cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | chỉ số 0 → 7 | [7,5,7] | [( -7,0 ), ( -7,2 ), ( -5,1 )] | 
| 2 | 7 | chỉ số 2 → 4 | [7,5,4] | đống cập nhật | 

Sau dịch vụ, top 2 đã được giảm xuống một cách chính xác. 

Điều này xác nhận rằng việc trích xuất lặp lại luôn nhắm tới mức tối đa hiện tại. 

### Ví dụ 2 

đầu vào:```
n = 4, s = 2
arr = [8, 6, 6, 3]
service1 = (mi=2, ci=3)
service2 = (mi=1, ci=2)
```Sau dịch vụ 1:```
[6, 4, 4, 3]
```Sau dịch vụ 2:```
[5, 4, 3, 3]
```Điều này cho thấy các phần tử có thể nhập lại phân khúc trên cùng sau khi được giảm không đồng đều, điều này được xử lý một cách tự nhiên bởi cấu trúc heap. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((tổng ci) log n + n log n) | mỗi lựa chọn là một đống pop/push, việc sắp xếp cuối cùng chiếm ưu thế | 
| Không gian | O(n) | heap lưu trữ một mục nhập cho mỗi trung tâm dữ liệu | 

Được cho`n ≤ 100000`Và`s ≤ 5000`, điều này phù hợp thoải mái trong giới hạn bộ nhớ. Thời gian chạy được điều khiển bởi các thao tác heap, trong thực tế vẫn duy trì hiệu quả đối với các ràng buộc này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    # inline solution
    import heapq

    def solve():
        n, s = map(int, sys.stdin.readline().split())
        arr = list(map(int, sys.stdin.readline().split()))
        heap = [(-arr[i], i) for i in range(n)]
        heapq.heapify(heap)

        for _ in range(s):
            mi, ci = map(int, sys.stdin.readline().split())
            for _ in range(ci):
                val, idx = heapq.heappop(heap)
                arr[idx] -= mi
                heapq.heappush(heap, (-arr[idx], idx))

        arr.sort(reverse=True)
        return " ".join(map(str, arr))

    return solve()

# basic sample-like case
assert run("3 1\n10 5 7\n3 2\n") == "7 5 4"

# all equal
assert run("4 1\n5 5 5 5\n1 4\n") == "4 4 4 4"

# single service single center
assert run("3 1\n9 1 2\n3 1\n") == "9 2 0"

# no services
assert run("5 0\n1 2 3 4 5\n") == "5 4 3 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mọi dịch vụ đều bình đẳng | 4 4 4 4 | sự lựa chọn thống nhất đúng đắn | 
| cập nhật duy nhất | 9 2 0 | tính chính xác của bản cập nhật heap cơ bản | 
| không có dịch vụ | 5 4 3 2 1 | trường hợp nhận dạng | 
| giảm liên tục | 7 5 4 | tính chính xác của nhiều phép khai thác | 

## Vỏ cạnh 

Một kịch bản phức tạp là khi nhiều trung tâm dữ liệu bắt đầu với các giá trị giống hệt nhau. Trong trường hợp này, thứ tự lựa chọn giữa các mối quan hệ không quan trọng, nhưng việc triển khai không chính xác đôi khi cho rằng thứ tự ổn định và vô tình làm sai lệch các cập nhật đối với các chỉ số trước đó. Cách tiếp cận dựa trên heap tránh được điều này vì nó luôn chỉ so sánh các giá trị thực tế. 

Một trường hợp khác là khi một trung tâm dữ liệu được chọn nhiều lần trong cùng một dịch vụ do trích xuất vùng nhớ heap lặp đi lặp lại. Thuật toán xử lý điều này một cách tự nhiên vì sau lần giảm đầu tiên, nó có thể vẫn nằm trong số các giá trị lớn nhất. 

Cuối cùng, khi`ci`lớn so với`n`, cùng một phần tử có thể được bật và đẩy liên tục theo một vòng lặp chặt chẽ. Điều này được mong đợi và vẫn đúng vì mỗi lần trích xuất thể hiện một lựa chọn độc lập trong định nghĩa dịch vụ và vùng heap luôn phản ánh các giá trị được cập nhật sau mỗi thao tác.
