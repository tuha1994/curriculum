### Giới thiệu

Congratulations, you have made it to the very đầu tiên React dự án. In this dự án, you sẽ be tạo a small ứng dụng nơi users có thể input their information và generate a CV/résumé. It's a great dự án to get you started vì it includes many of the cơ bản concepts you've đã học so far, không có a huge overhead of complexity.

### Bài tập

<div class="lesson-content__panel" markdown="1">

<div class="lesson-note" markdown="1">

#### Double kết xuất

While working on this dự án, you might notice that some parts of your mã execute twice (e.g., console logs appearing doubled). This is due to [React.StrictMode](https://react.dev/reference/react/StrictMode) và is intended behavior. chúng ta sẽ go qua this thêm in the tiếp theo bài học, so don't worry về it for bây giờ.

</div>

1. tạo a mới React dự án.
1. Think về cách to structure your ứng dụng vào các component. Your ứng dụng nên include:
    1. A section to add general information like name, email and phone number.
    1. A section to add your educational experience (school name, title of study and date of study)
    1. A section to add practical experience (company name, position title, main responsibilities of your jobs, date from and until when you worked for that company)
1. Be sure to include an edit và submit button for mỗi section hoặc for the whole CV. The submit button nên submit your form và display the giá trị of your input fields in HTML các phần tử. The edit button nên add back (display) the input fields, với the previously displayed information as các giá trị. In those input fields, Bạn nên be able to edit và resubmit the content. You're going to Tạo heavy sử dụng of state và props, so Tạo sure you understood those concepts.
1. tạo a `components` directory bên dưới your `src` directory và add your các component.
1. Include a `styles` directory bên dưới your `src` directory for your CSS các tệp. You'll cần to import these in the component các tệp to sử dụng them.
1. Push the results và deploy them với any of the options mentioned below. At this point of the curriculum, it doesn't matter which platform you choose as long as your dự án is live on the internet!

</div>

#### Deploying a React app

While we have been sử dụng GitHub Pages to deploy our **static webpages** till bây giờ, và chúng ta có thể definitely continue to do so với hacky ways, it's far easier to let a PaaS (Platform as a Service) do the giống nhau for us! ở đó are a lot of options to host such sites, và the [Vite hosting docs](https://vitejs.dev/guide/static-deploy.html) go over the most popular ones among them.

Not chỉ do some of these get rid of additional steps Sau khi pushing our mã to GitHub as these services have tools that automatically *deploy on push*, nhưng you cũng don't have to worry về khác issues further down the curriculum, like routers và Xây dựng steps. We shall sử dụng a few selected options for bây giờ. Sau khi you are done với the assignment, choose one of the following options, và deploy!

#### Netlify

While ở đó are many ways to deploy to [Netlify](https://www.netlify.com/) ([Netlify documentation](https://docs.netlify.com/)), like uploading your `dist` directly hoặc sử dụng `netlify-cli`, the most convenient way sẽ be to directly import your GitHub repository to Netlify.

1. Push your React ứng dụng to GitHub.
1. [Import your dự án to Netlify](https://app.netlify.com/start) by logging in, và selecting your repository.
1. Select the branch to deploy từ (the default setting, từ `main`, works) và hit "Deploy site"!
1. Bạn có thể access thêm settings ở đây if you cần to!

#### Vercel

Similar to Netlify, [Vercel](https://www.vercel.com/) ([Vercel documentation](https://www.vercel.com/docs)) cũng offers a few ways to deploy, và chúng ta sẽ cũng be importing our GitHub repository to get benefits like deploy-on-push.

1. Again, push your React ứng dụng to GitHub.
1. [Import your dự án to Vercel](https://vercel.com/new).
1. Vercel sẽ automatically detect that you are sử dụng Vite. Set your name as you like, và hit "Deploy"!

#### Cloudflare Pages

[Cloudflare Pages](https://pages.cloudflare.com/) ([Cloudflare Pages documentation](https://developers.cloudflare.com/pages)) has a similar process và benefits to the khác two options: import your GitHub repository và set the right Xây dựng command và output directory.

1. Yes, push your React ứng dụng to GitHub.
1. [Import your dự án to Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/deploy-a-vite3-project/#:~:text=To%20deploy%20your%20site%20to%20Pages). Tạo sure Khi tạo an ứng dụng that you tạo với Pages Thay vào đó of tạo a Worker.
1. Hit "Save và Deploy" và watch it come to life!
