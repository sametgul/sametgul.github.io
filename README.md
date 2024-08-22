# Welcome to My Personal Website Repository!

Hello! I'm Samet Gul, an engineer and researcher specializing in computer science, control, and robotics. I started this website in July 2024 as a digital space to showcase my projects, research endeavors, and knowledge. I’m passionate about exploring cutting-edge technologies and developing innovative engineering solutions. This platform is designed to share my work and make my study notes accessible, ensuring that my learning experiences benefit others as well.

## Description

This website was built using the [Chirpy Jekyll theme](https://github.com/cotes2020/jekyll-theme-chirpy) and is hosted on GitHub Pages. It serves as a portfolio to highlight my expertise, which spans computer science, control systems, and robotics, with strong proficiency in C/C++, Python, Object-Oriented Programming (OOP), Data Structures and Algorithms (DSA), and MATLAB/Simulink. I am also deeply interested in Machine Learning, Robot Operating System (ROS), and Embedded Systems. My primary focus is on integrating control systems and robotics with Machine Learning and Deep Learning to drive advancements in these fields.

## How to Clone and Set Up Your Environment

### Install Ruby and Other Prerequisites

For Debian-based systems (like Ubuntu), install Ruby and other required packages:

```sh
sudo apt-get install ruby-full build-essential zlib1g-dev git
```

Avoid installing RubyGems packages (gems) as the root user. Instead, set up a gem installation directory for your user account. The following commands will add environment variables to your `~/.bashrc` file to configure the gem installation path:

```sh
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Finally, install Jekyll and Bundler:

```sh
gem install jekyll bundler
```

### Cloning the Repository

Clone the repository to your local machine:

```sh
git clone https://github.com/sametgul/sametgul.github.io.git
cd sametgul.github.io
```

### Installing Dependencies and Setting Up the Project

Before running the local server for the first time, navigate to the root directory of your site and run:

```sh
bundle
```

### Running the Local Server

To preview the site contents before publishing, run:

```sh
bundle exec jekyll s
```

After a few seconds, the local service will be available at [http://127.0.0.1:4000](http://127.0.0.1:4000).

## Contact

Feel free to reach out to me for collaborations or inquiries:

- Email: [asam.gul@gmail.com](mailto:asam.gul@gmail.com)
- [LinkedIn](https://linkedin.com/in/gul-samet)
- [GitHub](https://github.com/sametgul)

Thank you for visiting my website!